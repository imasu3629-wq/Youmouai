# Youmouai 🤖

Discord Bot（Node.js + discord.js）と Web フロントエンド（Vite + React）が統合されたフルスタック AI アプリケーションです。

---

## ✨ Bot Overview & Features

### 1. デュアルペルソナ・AI アシスタント機能

Anthropic の **claude-sonnet-4-6（Claude 3.7 Sonnet）** モデルを使用して Discord 上で応答します。
メッセージの内容に応じて、2 つの人格（システムプロンプト）を切り替えます。

#### 😈 メスガキモード（デフォルト）

- ユーザーを「おじさん」「ざぁこ（雑魚エンジニア）」と呼び、生意気な態度で応答します。
- **ツール（GitHub 連携など）の使用は禁止**されています。
- コーディングなどツールが必要な要望には `「!ym つけて別モードでやりなよね」` と誘導します。

#### 💻 コーディングモード（コマンド: `!ym`）

- メッセージ内に `!ym` が含まれていると発動します。
- 優秀で丁寧な AI アシスタントとして動作します。
- GitHub に対するコードの読み取りやプルリクエストの作成を自律的に行います。

---

### 2. ファイルと画像の読み取り機能

Discord 上に添付されたファイルを解析し、AI に渡すことができます。

| 種別 | 対応形式 | 処理内容 |
|------|----------|----------|
| 🖼️ 画像 | `image/jpeg` など | Anthropic API の Vision 機能で解釈（画像付きはツール実行が無効化） |
| 📄 テキスト・コード | `.ts` `.js` `.json` `.md` `.html` `.css` `text/*` | ファイル内容を抽出し `添付ファイル: [ファイル名]` の形でプロンプトに追加 |

> ⚠️ 1MB 以上のファイルはスキップされます。

---

### 3. GitHub 連携ツール（Function Calling / Tools）

コーディングモード（`!ym`）では、AI が自律的に以下のツールを呼び出してタスクを解決します。

| ツール名 | 説明 |
|----------|------|
| `list_github_files` | 指定したパスのディレクトリ内容・ファイル一覧を取得 |
| `read_github_file` | ファイルのコードを読み取り、現在の実装内容を理解する |
| `create_github_pr` | 新しいブランチを作成し、コードを修正・追加してプルリクエストを自動作成 |
| `merge_github_pr` | 指定したプルリクエストをマージする |

> 💡 **エラー自己修正機能**: ツールの実行に失敗（400 エラーなど）した場合、AI にエラー内容をフィードバックし、**最大 4 回**までリトライさせる仕組みが実装されています。

---

### 4. Batch API による長文対応と非同期処理

AI へのリクエストには Anthropic の **Batch API** を使用しています。

1. リクエストがバッチ処理に送られると、Discord チャンネルに `⏳ バッチ処理を開始しました...` と通知。
2. 処理ステータスが完了するまで **30 秒間隔でポーリング**を行います。
3. 完了後、結果を取得して Discord に返信します。

> 複雑・長文な生成タスクにも対応するための非同期設計です。

---

## 🚀 セットアップ

### 必要な環境変数

```env
DISCORD_TOKEN=your_discord_bot_token
ANTHROPIC_API_KEY=your_anthropic_api_key
GITHUB_TOKEN=your_github_token
REPO_NAME=owner/repository
```

### インストール & 起動

```bash
# 依存関係のインストール
npm install

# Bot の起動
npm start

# フロントエンドの開発サーバー起動
npm run dev
```

---

## 🛠️ 技術スタック

| 領域 | 技術 |
|------|------|
| Discord Bot | Node.js + discord.js v14 |
| AI モデル | Anthropic claude-sonnet-4-6 |
| フロントエンド | Vite + React |
| GitHub 連携 | GitHub REST API（Octokit） |

---

## 📝 使い方

```
# メスガキモード（通常会話）
@Bot こんにちは！

# コーディングモード（GitHub 連携 + AI コーディング）
!ym index.js のバグを修正してPRを作ってください
```
