# Youmouai 🤖

Discord Bot (Node.js + discord.js) と Web フロントエンド (Vite + React) が統合されたフルスタック AI アシスタントアプリです。

## ✨ Features

### 1. デュアルペルソナ・AI アシスタント

| モード | トリガー | 説明 |
|---|---|---|
| 😈 メスガキモード | デフォルト | ユーザーを「おじさん」「ざぁこ」と呼ぶ生意気なキャラ。ツール使用不可。 |
| 💻 コーディングモード | `!ym` を先頭に付ける | 優秀で丁寧なアシスタントとして GitHub 連携ツールを駆使してタスクを解決。 |

AI モデルは **Anthropic claude-sonnet-4-6 (Claude 3.7 Sonnet)** を使用しています。

### 2. ファイル・画像の読み取り

- **画像** (`image/jpeg` など): Vision 機能で解釈（画像添付時はツール無効）
- **テキスト / コード** (`.ts` `.js` `.json` `.md` `.html` `.css` `text/*`): 中身を抽出してプロンプトに追加（1 MB 超はスキップ）

### 3. GitHub 連携ツール（コーディングモード限定）

| ツール | 説明 |
|---|---|
| `list_github_files` | 指定パスのファイル一覧を取得 |
| `read_github_file` | ファイルの内容を読み込む |
| `create_github_pr` | ブランチ作成 → コード修正 → PR 自動作成 |
| `merge_github_pr` | 指定 PR をマージ |

ツール実行失敗時（400 エラーなど）は AI にフィードバックして **最大 4 回**まで自己修正リトライします。

### 4. Batch API による長文対応・非同期処理

Anthropic の **Batch API** でリクエストを送信し、完了まで **30 秒間隔でポーリング**します。
処理中は Discord チャンネルに `⏳ バッチ処理を開始しました...` と通知されます。

## 🚀 セットアップ

### 必要な環境変数

```env
DISCORD_TOKEN=your_discord_bot_token
ANTHROPIC_API_KEY=your_anthropic_api_key
GITHUB_TOKEN=your_github_personal_access_token
REPO_NAME=owner/repository
```

### インストール & 起動

```bash
npm install
npm start
```

### フロントエンド (Vite + React)

```bash
cd client
npm install
npm run dev
```

## 📁 ディレクトリ構成

```
.
├── index.js          # Discord Bot メインエントリ
├── package.json
├── client/           # Vite + React フロントエンド
│   ├── index.html
│   ├── src/
│   │   ├── main.jsx
│   │   └── App.jsx
│   └── package.json
└── README.md
```
