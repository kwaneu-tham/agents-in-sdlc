---
title: "レッスン 1 - GitHub Copilot CLI をインストールする"
description: "Codespace に Copilot CLI をインストールして認証し、基本操作を確認して、登録済みのフィルター機能の Issue を見つけます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] は、ターミナルで動作するエージェント型コーディングアシスタントです。コードベースを調べ、コードを生成し、コマンドを実行し、外部ツールを利用できます。作業を任せたり、変更を依頼したりしながら、ターミナルで作業を続けられます。まずは Copilot CLI をインストールします。GitHub Codespaces には、使い慣れたツールがあらかじめ用意されています。

このレッスンでは、次の内容を学習します。

- npm を使って GitHub Copilot CLI をインストールする。
- GitHub アカウントで認証する。
- ワークショップのリポジトリを信頼し、簡単な会話を試す。
- 組み込みの GitHub MCP サーバーでフィルター機能の Issue を見つける。

## シナリオ

チームでは、増え続けるバックログに取り組むために AI エージェントを使い始めています。Copilot CLI は、多くの開発者が日常的に使うターミナルでその機能を利用できるようにします。このレッスンでは、インストールと認証を済ませ、ワークショップで使える状態にします。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew でインストールできます。GitHub Codespaces には Node.js があらかじめインストールされているため、この演習では npm を使います。

1. Codespace に戻り、ターミナルを開きます。
2. Node.js がインストールされ、必要なバージョンを満たしていることを確認します。

   ```bash
   node --version
   ```

   バージョン 24 以上が表示されます。

3. Copilot CLI をグローバルにインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. インストールを確認します。

   ```bash
   copilot --version
   ```

   バージョン番号が表示されます。

## GitHub で認証する

Copilot CLI を初めて起動すると、GitHub アカウントでの認証が求められます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. 求められた場合は、デバイスコードの案内に従って認証し、Copilot CLI のアクセスを許可します。
3. Copilot CLI に次のプロンプトが表示されることを確認します。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. 表示されたパスが Tailspin Toys リポジトリであることを確認し、**Yes, and remember this folder for future sessions** を選択します。

> [!NOTE]
> Codespace では GitHub セッションですでに認証されている場合があります。認証を求められずに Copilot CLI が起動した場合は、そのまま進めてください。

## 基本操作を確認する

通常のシェルプロンプトで入力したコマンドは、Codespace で実行されます。Copilot CLI の起動後は、自然言語をエージェントに送り、スラッシュコマンドで会話を操作します。

1. `/model` を入力し、矢印キーで **Auto** を選択して <kbd>Enter</kbd> を押します。もう一度 <kbd>Enter</kbd> を押して確定します。
2. `/help` を入力して現在のバージョンで使用できるコマンドを確認し、<kbd>Esc</kbd> でヘルプ画面を閉じます。
3. 次の質問で、Copilot が正しく動作することを確認します。

   ```plaintext
   このプロジェクトの主要なファイルは何ですか？
   ```

4. 応答を読み、Copilot が回答前にリポジトリを調べていることを確認します。
5. `/mcp list` を入力し、組み込みの GitHub MCP サーバーが利用できることを確認します。
6. 次のプロンプトでフィルター機能の Issue を探します。

   ```plaintext
   GitHub MCP を使って、このリポジトリで「Allow users to filter games by category and publisher」というタイトルの Issue を探してください。URL と短い概要を教えてください。何も変更しないでください。
   ```

7. 表示された URL を開き、Issue を読みます。最初の小さな変更を終えた後に、この Issue に取り組みます。

> [!TIP]
> 通常の Copilot CLI セッションは、ターミナルで現在チェックアウトされているブランチを使います。自動的に worktree を作成しません。変更を始める前に、機能ブランチを作成します。

## ワークショップ用のショートカットを使う

Copilot CLI は、許可された権限の範囲外でツールを使う前に確認を求めます。このワークショップでは、Codespace 内の承認プロンプトをなくし、演習に集中するため、ユーザーが許可したショートカット `--yolo` を使います。

> [!CAUTION]
> `--yolo` は、`--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls` を含む完全な自動権限を有効にします。Codespace や VM などの隔離された環境でのみ使い、日常の開発で既定の設定としてエイリアスにしないでください。詳細は[ツール使用の許可と拒否][allow-all-warning]を参照してください。

このワークショップで `--enable-all-github-mcp-tools` を指定すると、後のレッスンで Issue やプルリクエストを扱うための GitHub MCP 読み取り・書き込みツールが有効になります。Codespace はローカルコンピューターへのアクセスを制限しますが、認証された GitHub リソースには実際にアクセスします。変更を公開またはマージする前にレビューしてください。

1. `/exit` で Copilot CLI を終了します。
2. リポジトリのルートから再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. プロジェクトについて簡単な質問をもう一度送り、会話が動作することを確認してから `/exit` で終了します。

Copilot は会話を自動的に保存します。後で指示を変更したりエージェントを追加したりした後に、`copilot --resume` で同じ機能ブランチの会話に戻ります。

## まとめと次のステップ

このレッスンでは、次の作業を行いました。

- npm を使って GitHub Copilot CLI をインストールした。
- GitHub アカウントで認証した。
- ワークショップのリポジトリを信頼し、簡単な会話を試した。
- 組み込みの GitHub MCP サーバーでフィルター機能の Issue を見つけた。

次は、[最初の対象を絞った変更][next-lesson]を開始し、Copilot CLI でゲームカードに星評価を表示します。

## リソース

- [GitHub Copilot CLI のインストール][install-cli]
- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
