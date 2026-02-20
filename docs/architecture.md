# アーキテクチャ設計書

## 採用アーキテクチャ
**Feature-first + Clean Architecture**

各機能 (Feature) ごとに独立したモジュールとして構成し、
それぞれが Data / Domain / Presentation の3層構造を持つ。

---

## フォルダ構成

```
lib/
├── main.dart                    # アプリエントリーポイント
│
├── core/                        # アプリ全体で共有されるコード
│   ├── constants/               # 定数定義
│   │   └── app_constants.dart
│   ├── theme/                   # テーマ設定
│   │   └── app_theme.dart
│   ├── utils/                   # ユーティリティ関数
│   ├── extensions/              # Dart拡張メソッド
│   └── widgets/                 # 共通ウィジェット
│
├── router/                      # ルーティング設定
│   └── app_router.dart          # GoRouterの設定
│
├── l10n/                        # 多言語化リソース
│   ├── app_ja.arb               # 日本語
│   ├── app_en.arb               # 英語
│   ├── app_zh.arb               # 中国語（簡体字）
│   └── generated/               # 自動生成ファイル
│
└── features/                    # 機能別モジュール
    │
    ├── auth/                    # 認証・ロール選択機能
    │   ├── data/
    │   │   ├── datasources/     # データソース（API、ローカルDB）
    │   │   └── repositories/    # リポジトリ実装
    │   ├── domain/
    │   │   ├── entities/        # エンティティ（ビジネスモデル）
    │   │   ├── repositories/    # リポジトリインターフェース
    │   │   └── usecases/        # ユースケース
    │   └── presentation/
    │       ├── providers/       # Riverpodプロバイダー
    │       ├── screens/         # 画面ウィジェット
    │       └── widgets/         # 機能固有ウィジェット
    │
    ├── home/                    # ホーム画面（ゲージ表示）
    │   ├── data/
    │   ├── domain/
    │   └── presentation/
    │
    ├── monitoring/              # 監視ロジック
    │   ├── data/
    │   ├── domain/
    │   └── presentation/
    │
    ├── pairing/                 # ペアリング機能
    │   ├── data/
    │   ├── domain/
    │   └── presentation/
    │
    └── settings/                # 設定画面
        ├── data/
        ├── domain/
        └── presentation/
```

---

## 各層の責務

### Data層
- **datasources/**: 外部データソースとの通信（API、SharedPreferences、MethodChannel等）
- **repositories/**: Domain層で定義されたリポジトリインターフェースの具体的な実装

### Domain層
- **entities/**: ビジネスロジックで使用するデータモデル
- **repositories/**: データ操作の抽象インターフェース
- **usecases/**: 単一責任のビジネスロジック

### Presentation層
- **providers/**: Riverpodによる状態管理
- **screens/**: 画面単位のウィジェット
- **widgets/**: 画面内で使用する部品ウィジェット

---

## 依存関係ルール

```
Presentation → Domain ← Data
     ↓            ↑
     └────────────┘
         (via DI)
```

- **Presentation** は **Domain** に依存
- **Data** は **Domain** に依存
- **Domain** は他の層に依存しない（Pure Dart）
- 依存性注入 (DI) は Riverpod を使用

---

## 使用パッケージ

| カテゴリ | パッケージ | 用途 |
|---------|-----------|------|
| 状態管理 | flutter_riverpod, riverpod_annotation | 状態管理・DI |
| ナビゲーション | go_router | 宣言的ルーティング |
| ローカル保存 | shared_preferences | 設定値の永続化 |
| 多言語化 | flutter_localizations, intl | i18n対応 |
| UI | google_fonts, gap | フォント・レイアウト |
| コード生成 | riverpod_generator, build_runner | Riverpodコード生成 |









