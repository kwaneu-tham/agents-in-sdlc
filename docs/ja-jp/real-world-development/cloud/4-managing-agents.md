---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

これまでの演習で、ユーザー体験の改善と機能追加を目的とする3つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも重要です。

GitHub.com の[エージェントページ][agents-page]など、cloud agent に割り当てたタスクを管理するツールがあります。このコントロールセンターでは、オープンなプルリクエスト (PR) があるエージェントのタスクを確認できます。実行内容を調べ、進行中のセッションに指示を追加することもできます。

このレッスンでは、次の内容を学習します。

- エージェントページで cloud agent のタスクを監視する。
- 実行中のセッションに追加機能を依頼する。

## シナリオ

エージェントにハイコントラストモードを依頼した後、チームはライトモードも追加したいと考えました。サイトのスタイルと切り替え機能を更新しているため、同じ作業に含めるのが合理的です。エージェントにライトモードも追加するよう指示します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状態を確認します。

1. [Copilot エージェントページ](https://github.com/copilot/agents)を開きます。
2. メインペインと左ペインのタスクリストを確認します。次のようなタスクが表示されます。
   - コードベースのドキュメントを更新する。
   - ゲーム変更用のアプリケーションプログラミングインターフェイス (API) を生成する。
   - Web サイトにハイコントラストモードを追加する。
3. 実行中のタスクを選択し、Copilot が行った作業を確認します。リポジトリのチェックアウト、作業環境や MCP サーバーの設定、タスク完了に向けた各種操作などが含まれます。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や進め方によって異なります。

4. 右側のプルリクエスト (PR) ペインを確認します。PR と変更されたファイルを確認できます。

## Cloud agent に作業中の指示を出す

進行中のタスクを確認したので、ハイコントラストモードの作業中にライトモードも追加するよう Copilot に依頼します。

1. ハイコントラストモードのタスクに対応するセッションを選択します。タイトルは Copilot が付けた名前と作業状況によって異なります。

    ![コントロールセンターのアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが終わり、作業を開始したことが表示されるまで数分待ちます。作業開始後は、次のようなメッセージが表示されます。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![エージェントページで、Steer active session while Copilot is working ダイアログを示すスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを確認し、作業計画に反映することを確かめます。

## Copilot の作業を待つ

Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で作業中の手順を終えた後、新しい依頼を反映します。

作業には数分かかります。待っている間に、これまでに学んだことを振り返ってください。

## まとめと次のステップ

Copilot のエージェントページを使って、cloud agent のタスクを監視し、作業中のセッションに指示を追加しました。このコントロールセンターでは、進行中や完了済みのタスクを追跡し、作業内容を確認し、範囲の変更や追加のガイダンスを伝えられます。

このレッスンでは、次の内容を学びました。

- エージェントページを使って cloud agent のタスクを監視した。
- 作業中のセッションに追加機能を依頼した。

Copilot がアクセシビリティ機能を実装する間に、次は[Copilot が作成したプルリクエストを反復的に改善します][next-lesson]。

## リソース

- [Copilot エージェントページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
