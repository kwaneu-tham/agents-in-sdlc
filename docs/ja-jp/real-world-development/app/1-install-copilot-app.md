---
title: "レッスン 1 - GitHub Copilot app をインストールする"
description: "GitHub Copilot app をインストールし、テンプレートから作成したリポジトリを接続して、ワークスペースを確認し、クイックチャットを試します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app][about-copilot-app]** は、エージェント主導の開発のためのデスクトップアプリケーションです。GitHub Copilot CLI を基盤とし、GitHub とネイティブに統合されているため、リポジトリ、ブランチ、CI パイプラインをすぐに利用できます。自分だけで作業するのではなく、複数のエージェントをそれぞれ分離されたワークスペースで並列に指示し、繰り返し作業を自動化するワークフローに適しています。Node.js のインストールとプロジェクトのコピーが完了したので、アプリをインストールしてリポジトリを接続します。

このレッスンでは、次の内容を学習します。

- GitHub Copilot app をインストールしてサインインする。
- GitHub リポジトリからプロジェクトをアプリに追加する。
- テンプレートで用意されたバックログを含め、ワークスペースを確認する。
- クイックチャットでアプリについて質問する。

## シナリオ

チームは、増え続けるバックログに取り組むために AI エージェントを導入しています。Copilot app を使うと、Issue の選択、エージェントの実行、変更のレビュー、プルリクエストのマージを一か所から指示できます。このレッスンでは、アプリをインストールして接続し、プロジェクトについての会話を始められるようにします。

> [!NOTE]
> 対象となる Copilot プランが必要です。Copilot Free、Copilot Student、または有料プラン (Pro、Pro+、Business、Enterprise) を利用してください。Copilot Business または Copilot Enterprise の場合、管理者が **GitHub Copilot app** ポリシーを有効にしている必要があります。

## GitHub Copilot app をインストールして設定する

GitHub Copilot app は Windows、macOS、Linux で利用できます。アプリをインストールして認証し、Tailspin Toys リポジトリを接続します。

1. ブラウザーで [GitHub Copilot app のランディングページ][download-app]を開きます。
2. 使用するプラットフォーム向けのアプリをダウンロードし、ページの手順に従ってインストールします。
3. インストール後にアプリを開きます。
4. **Sign in to GitHub** を選択し、画面の指示に従って GitHub.com または GHE.com アカウントでサインインします。データ所在地に対応する GitHub Enterprise Cloud を使う場合は **GitHub Enterprise Cloud** を選択し、求められたら `*.ghe.com` のホスト名を入力します。

   GitHub Enterprise Server を使う場合は、まずクラウドアカウントでオンボーディングを完了します。その後、**Settings** を開き、**Accounts**、**Add account**、**GitHub Enterprise Server** の順に選択します。[Enterprise Server のサインインガイド][ghes-sign-in]に従い、ブラウザーでアプリを承認します。

5. オンボーディング中にリポジトリの選択が表示された場合は、テンプレートから作成した Tailspin Toys リポジトリを選択します。リポジトリの選択をスキップし、オンボーディング後に追加することもできます。
6. テーマの選択を求められたら選択し、オンボーディングを完了します。
7. サイドバーの **Sessions** でリポジトリを確認します。すでに一覧にあれば、再度追加する必要はありません。
8. リポジトリがまだ接続されていない場合は、**Sessions** の横にある **+** を選択して新しいセッションを作成し、**Add GitHub repository** を選択します。前提条件のレッスンで控えた所有者名とリポジトリ名を使います。
9. リポジトリがサイドバーに表示されることを確認します。このレイアウトでは `tailspin-toys` のようなリポジトリフォルダーが **Sessions** の下にまとまり、その下に個々のセッションが表示されます。

## ワークスペースを確認する

プロジェクトを接続したら、サイドバーの領域を確認します。

- **New** - Copilot との新しいチャットセッションを開始します。
- **Pull requests** - アプリの GitHub 統合を通じてプルリクエストを表示します。アプリを離れずに一覧の確認や絞り込み、CI 状態の確認、レビューを行えます。
- **Issues** - Issue を確認して絞り込み、Issue からセッションを開始できます。
- **Automations** - スケジュールまたはオンデマンドで実行する、保存済みのエージェントタスクです。ToDo リストの管理、定期的な保守などを任せられます。まとめで次のステップとして紹介しますが、別の演習ではありません。
- **Customize** - MCP サーバー、プラグイン、スキルなどのコンポーネントを追加して Copilot app を拡張します。Playwright MCP の設定に使います。
- **Chats** - 独自のブランチやワークスペースを必要としない、質問やブレインストーミング向けの簡易的な会話です。最初のチャットを開始すると表示されます。表示されていない場合は、**Sessions** の横にある **+** を選択して最初のチャットを開始します。
- **Sessions** - リポジトリフォルダーとそのエージェントセッションをまとめます。`tailspin-toys` のようなリポジトリ名を選ぶとリポジトリビューが開き、その下のセッションを選ぶと会話に戻ります。各セッションは分離されたワークスペースで動くため、変更を競合させずに複数のセッションを実行できます。星評価を追加するレッスンで最初のセッションを開始します。

ワークショップを進めながら、ワークスペースの機能を確認します。

> [!TIP]
> 迷ったときは Copilot に質問してください。操作方法が分からない場合や、何かが可能か知りたい場合に案内してくれます。

### 用意されたバックログを確認する

テンプレートからリポジトリを作成すると、バックログも用意されます。次のタイトルを使って確認します。

1. サイドバーの **Sessions** で Tailspin Toys のリポジトリフォルダーを選択します。フォルダーの下にある個別のセッションではありません。次に、リポジトリ上部のナビゲーションで **Issues** を選択します。
2. Issue 番号を決めつけず、次のタイトルを探します。
   - Allow users to filter games by category and publisher
   - Update our repository coding standards
3. Issue を選択して内容を読みます。各 Issue からエージェントセッションを開始できます。最初の小さな変更を終えた後、フィルター機能の Issue から作業します。

> [!NOTE]
> **Issues** と **Pull requests** の一覧は、Copilot app に追加したリポジトリの項目だけを表示するよう自動的に絞り込まれます。ほかのリポジトリの作業項目を表示するには、そのリポジトリをアプリに追加してください。

## クイックチャットを試す

アプリ自体について質問すると、アプリに慣れることができます。**quick chat** では、ブランチや worktree を作成せずに質問やブレインストーミングを行えます。セッションを必要としない、その場限りの質問に適しています。

1. サイドバーで **Chats** の横にある **+** を選択し、新しいチャットを開きます。**Chats** がまだ表示されていない場合は、**Sessions** の横の **+** を選択して最初のチャットを開始します。その後 **Chats** が表示されます。
2. 次の質問で、アプリのセッションの仕組みを尋ねます。

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. 会話ビューで回答を読みます。各セッションは分離された独自の git worktree で実行されます。これにより、変更を競合させずに複数のエージェントを並列実行できます。会話は続けることも、新しいチャットを開始することもできます。

## まとめと次のステップ

GitHub Copilot app をインストールし、プロジェクトを接続してワークスペースを確認しました。具体的には、次の作業を行いました。

- アプリをインストールして GitHub にサインインした。
- GitHub リポジトリからプロジェクトを追加した。
- ワークスペースを確認し、**Issues** で用意されたバックログを見つけた。
- クイックチャットで、その場限りの簡単な質問をした。

次は、[最初のエージェントセッションを開始し][next-lesson]、ゲームカードに星評価を表示します。

## リソース

- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app の概要][getting-started]
- [GitHub Copilot app でのエージェントセッションの操作][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
[ghes-sign-in]: https://docs.github.com/enterprise-server@3.22/copilot/copilot-on-ghes/sign-in-on-copilot-app
