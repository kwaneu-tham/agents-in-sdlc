---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

多くの組織は、未解決のセキュリティ問題、更新が必要なレガシーコード、時間がなくバックログに残った機能要望など、技術的負債を抱えています。GitHub Copilot の cloud agent は、コードの更新や機能追加などのタスクを自律的に実行できます。作業が終わると、開発者がレビューできるドラフトのプルリクエスト (PR) を作成します。手間のかかるタスクを任せることで、開発者はより大きな課題に集中できます。

この演習では、Copilot cloud agent を使って次の内容を学びます。

- コード生成の環境をカスタマイズする。
- 操作を安全に実行する。
- 範囲を明確に定めた Issue の重要性を理解する。
- Issue を Copilot に割り当てる。

## シナリオ

Tailspin Toys には、改善したい技術的負債があります。最初のサイトを作成した開発者が、ドキュメントをほとんど残しませんでした。まず、アプリケーション内のすべてのエクスポート関数に TSDoc ドキュメントコメントを追加します。

また、デザインチームはゲームを見つけやすくしたいと考えています。ゲーム詳細ページに、同じカテゴリーの関連ゲームを表示し、利用者がほかのゲームも閲覧できるようにします。まずは UX の受け入れテストに使える簡単な実装を希望しています。この作業は現在のブロッカーですが、優先度の高い Issue がほかにあります。

どちらも後回しになりやすいタスクです。Copilot cloud agent に割り当てれば、非同期で作業している間に別の作業を進め、後から結果をレビューできます。

## GitHub Copilot cloud agent とは

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドでタスクを実行できます。[GitHub Issue を Copilot に割り当てる][assign-issue]方法などがあります。割り当てると、Copilot は進行状況を追跡するドラフト PR を作成し、環境を準備してタスクに取り組みます。作業中でも完了後でもセッションを確認できます。提案された解決策のレビュー準備ができると、PR で通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単な作業でも、詳細を確認すると複雑なことがあります。[Copilot cloud agent のタスク範囲を慎重に定め][cloud-agent-best-practices]てください。AI ペアプログラマーとして段階的に進め、学び、試し、必要に応じて調整します。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と Copilot への影響を確認しました。まだ `.github/copilot-instructions.md` を見ていない場合は、cloud agent に作業を依頼する前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認してください。

github.com のリポジトリまたは起動中の Codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md`: **Code standards** セクション、特に TypeScript の規約と TSDoc ドキュメントコメントの要件を確認します。
- `.github/instructions/unit-tests.instructions.md`: `**/*.test.ts` に適用される `applyTo` フロントマターを確認します。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てたら、作成された PR に TSDoc コメント、TypeScript の規約、コメントヘッダーが反映されているか確認します。後の演習でも PR をレビューするときに確認します。

## Copilot cloud agent の開発環境を設定する

コードの作成には、環境やセットアップスクリプトが必要です。Copilot にタスクを割り当てる場合も、ソフトウェアエンジニアのように作業するための準備が必要です。

Cloud agent は、作業環境に [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に設定する[専用のセットアップワークフロー][setup-workflow]で、作業開始前に必要なツールや依存関係を準備できます。この演習では、Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、単一 Astro アプリ用にマイグレーションとシードを済ませた SQLite データベースを利用できるよう、あらかじめ設定しています。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # *手順* に必要な最小限の権限を設定します。Copilot の操作には専用のトークンが使用されます。
    permissions:
      # 依存関係のインストールなど、セットアップ手順でリポジトリをクローンする場合は `contents: read` 権限が必要です。セットアップ手順でクローンしない場合は、手順の完了後に Copilot が自動でクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテストでデータを使えるように、ローカル SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

このファイルは一般的な GitHub ワークフローと同じ形式ですが、次の点に注意してください。

- `copilot-setup-steps` という単一のジョブがあります。Copilot が PR に取り組む前に、GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot の実行を待たずに、正常に動作するか確認できます。

## ドキュメントを追加する

多くのプロジェクトでは、ドキュメントが古いか不足しています。こうした技術的負債は放置されやすく、生産性を下げ、コードベースの保守や新しい開発者の参加を難しくします。Copilot はドキュメントの作成に役立つため、cloud agent に依頼するのに適したタスクです。作業中はバックグラウンドで進められ、後の演習で結果をレビューできます。

1. 新しいブラウザータブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して割り当てダイアログを開きます。

    ![Copilot を Issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成された PR を開きます。タイトルは `[WIP]: Code lacks documentation` のような名前になります。表示されない場合は少し待ってからブラウザーを更新します。
12. 数分後、Copilot が ToDo リストを作成したことを確認します。

> [!NOTE]
> Copilot が ToDo リストを作成するまで数分かかる場合があります。前述のワークフローで環境を準備し、プロジェクトを分析して、最適な進め方を判断しているためです。

13. リストと、Copilot が実行するタスクを確認します。
14. PR のタイムラインを下にスクロールし、Copilot が Issue の作業を開始したことを確認します。
15. **View session** ボタンを選択します。

    ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 表示が更新されない場合は、ウィンドウを更新してください。

16. ライブセッションをスクロールし、Copilot がコードを調べて状態を理解し、計画を立て、コードを作成する様子を確認します。

作業には数分かかることがあります。cloud agent の利点は非同期で作業を進められることです。別の作業に取り組むため、もう1つのタスクも Copilot cloud agent に割り当てます。

## ゲーム詳細ページに関連ゲームを追加する

Copilot cloud agent に作業を分担できるため、別のタスクに取り組んでいる間に、デザインチーム向けの関連ゲーム機能を実装させます。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して割り当てダイアログを開きます。

    ![Copilot を Issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、最初のコメントに 👀 が表示され、Copilot が作業を始めたことを示します。

![目の絵文字で Copilot が Issue に取り組んでいることを示す画面](../../../_images/ex4-issue-eyes-emoji.png)

Copilot が2つ目のタスクに取り組んでいる間、作業を常に監視する必要はありません。完了後に結果をレビューできます。次は、カスタムエージェントの作成と使用を確認します。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents] を使い、非同期の Issue 割り当てを確認しました。cloud agent は技術的負債への対応、新機能の作成、フレームワーク間のコード移行などに使えます。

この演習では、次の内容を学びました。

- コード生成環境をカスタマイズした。
- 操作を安全に実行した。
- 範囲を明確に定めた Issue の重要性を確認した。
- Issue を Copilot に割り当てた。

cloud agent がバックグラウンドで作業する間に、次はカスタムエージェントを作成して使います。[Copilot cloud agent は MCP サーバーも利用でき][cloud-agent-mcp]、前の演習で確認したカスタム指示にも対応しています。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
