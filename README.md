# Youmouai 🤖

Discord Bot + Web フロントエンド統合のフルスタック AI アシスタントです。

---

## Bot Overview & Features

### 概要

このアプリケーションは **Discord Bot（Node.js + discord.js）** と **Web フロントエンド（Vite + React）** が統合されたフルスタックアプリケーションです。  
AI エンジンには **Anthropic claude-sonnet-4-6（Claude 3.7 Sonnet）** を使用しており、GitHub 連携・画像解析・非同期バッチ処理など多彩な機能を備えています。

---

## 主な機能

### 1. デュアルペルソナ・AI アシスタント

2 つの人格（システムプロンプト）を持ち、メッセージに応じて自動で切り替わります。

#### 😈 メスガキモード（デフォルト）
- ユーザーを「おじさん」「ざぁこ（雑魚エンジニア）」と呼び、生意気な態度で応答します。
- ツール（GitHub 連携など）の使用は**禁止**されています。
- コーディングなどツールが必要な要望には `「!ym つけて別モードでやりなよね」` と誘導します。

#### 💻 コーディングモード（コマンド: `!ym`）
- メッセージ内に `!ym` が含まれていると発動します。
- 優秀で丁寧な AI アシスタントとして動作します。
- GitHub に対するコードの読み取りやプルリクエストの作成を自律的に行います。

---

### 2. ファイルと画像の読み取り機能

Discord に添付されたファイルを解析して AI に渡します。

| ファイル種別 | 対応内容 |
|---|---|
| 画像（image/jpeg など） | Anthropic API の Vision 機能で解釈（画像付きメッセージはツール実行無効） |
| テキスト・ソースコード（.ts, .js, .json, .md, .html, .css, text/* など） | ファイル内容を抽出し `「添付ファイル: [ファイル名]」` としてプロンプトへ追加 |

> ⚠️ 1MB 以上のファイルはスキップされます。

---

### 3. GitHub 連携ツール（Function Calling / Tools）

`!ym` コーディングモードで AI が自律的に以下のツールを呼び出してタスクを解決します。

| ツール名 | 説明 |
|---|---|
| `list_github_files` | 指定パスのディレクトリ内容・ファイル一覧を取得 |
| `read_github_file` | コードを読み取り、現在の実装内容を理解 |
| `create_github_pr` | 新ブランチを作成し、コードを修正・追加してプルリクエストを自動作成 |
| `merge_github_pr` | 指定したプルリクエストをマージ |

> 🔄 ツールの実行に失敗（400 エラーなど）した場合、AI にエラー内容をフィードバックして**最大 4 回**までリトライします。（エラーを自己修正しようと試みます）

---

### 4. Batch API による長文対応と非同期処理

AI へのリクエストには **Anthropic の Batch API** を使用しています。

- バッチ処理が開始されると Discord チャンネルに `「⏳ バッチ処理を開始しました...」` と表示されます。
- ステータスが完了するまで **30 秒間隔でポーリング**を行い、結果を取得します。
- 時間がかかる複雑な生成タスクに対応するための仕組みです。

---

## 環境変数

| 変数名 | 説明 |
|---|---|
| `DISCORD_TOKEN` | Discord Bot のトークン |
| `ANTHROPIC_API_KEY` | Anthropic API キー |
| `GITHUB_TOKEN` | GitHub Personal Access Token |
| `REPO_NAME` | 操作対象のリポジトリ名（例: `owner/repo`） |

---

## 技術スタック

- **Bot**: Node.js / discord.js
- **AI**: Anthropic claude-sonnet-4-6（Claude 3.7 Sonnet）
- **フロントエンド**: Vite + React
- **GitHub 連携**: GitHub REST API（Octokit）
- **非同期処理**: Anthropic Batch API
