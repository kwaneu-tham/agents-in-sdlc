---
title: "レッスン 12 - GitHub Pages でサイトを公開する"
description: "GitHub Copilot app を使って Pages ワークフローを設定し、プルリクエストをレビューして、公開された Space Quiz を確認します。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

GitHub Pages にデプロイして、Space Quiz を公開しましょう。Copilot はワークフローを準備し、デプロイを案内できますが、変更をレビューし、サイトが公開されたことを確認するのは開発者の役割です。

このレッスンでは、次の内容を学習します。

- Copilot に GitHub Pages のデプロイ設定を依頼する。
- マージ前にワークフローとプルリクエストをレビューする。
- デプロイの進行を確認し、公開されたサイトを検証する。

## シナリオ

Space Quiz はすでに GitHub リポジトリにありますが、手軽に試す方法が必要です。単一ファイルのサイトを GitHub Pages で公開すれば、サーバーやランタイム依存関係を追加せずに公開 URL から利用できます。

## Copilot にデプロイの準備を依頼する

次のプロンプトを送る前に、Copilot app の `space-quiz` セッションで実行される操作を確認します。

> [!WARNING]
> プルリクエストの作成によって、コードがコミットおよびプッシュされることがあります。プロンプトを送信する前に、承認設定と送信先リポジトリを確認してください。**Approve all** を有効にしている場合、個別の確認は表示されないことがあります。

Copilot にデプロイの準備を依頼するには、次の操作を行います。

1. Copilot app の `space-quiz` セッションで、次のプロンプトを送信します。

    ```plaintext
    Prepare a GitHub Pages deployment for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages after changes reach the default branch. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and report its link and any required Pages settings or permissions. Do not merge, enable auto-merge, change Pages settings, or deploy. Stop and wait for my review.
    ```

## ワークフローとプルリクエストをレビューする

マージ前にワークフローの内容とプルリクエストの変更を確認し、デプロイが既定のブランチから実行されることを確かめます。

1. リポジトリに README がない場合に限り、README が追加されていることを確認します。
2. ビルドとデプロイの手順、トリガー、権限を含め、プルリクエストの変更をレビューします。
3. ワークフローが、未マージのプルリクエストではなく既定のブランチからデプロイすることを確認します。
4. 修正が必要な場合は依頼し、更新後の差分をレビューします。マージを承認する前に、必須のプルリクエストチェックが成功するまで待ちます。

## マージとデプロイを承認する

> [!WARNING]
> 以下のプロンプトは、レビュー済みのプルリクエストのマージ、リポジトリの Pages 設定の変更、サイトの公開をエージェントに許可します。レビューを完了し、リポジトリとプルリクエストを確認した後にのみ送信してください。承認設定によっては、これらの操作に別途確認が求められないことがあります。

次のプロンプトは、レビュー済みのプルリクエストのみを対象にします。`<PR_NUMBER>` を確認した番号に置き換えてから送信します。

1. 承認する対象のリポジトリとプルリクエストを確認してから、次のプロンプトを送信します。

    ```plaintext
    I have reviewed pull request <PR_NUMBER> in this repository and authorize its merge and deployment. Enable GitHub Pages using GitHub Actions, merge that pull request only after required checks pass, and monitor the deployment until it succeeds. Report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
    ```

![3段階の図。マージ済みの space-quiz プルリクエスト、Build website、Upload artifact、Deploy to Pages の各ステップが成功した GitHub Actions の Deploy website 実行、yourname.github.io/space-quiz/ で公開された Space Quiz を示しています。](../../../_images/first-steps-app-pages-deploy.svg)

## デプロイを確認する

GitHub Actions の実行結果と公開されたサイトの両方を確認し、デプロイが成功したことを確かめます。

1. GitHub Actions のデプロイ実行が成功するまで確認します。
2. 報告された URL を開き、Space Quiz が表示されることを確認します。

エージェントが Pages の有効化やプルリクエストのマージを実行できない場合は、案内された手順に従います。デプロイが成功し、公開サイトが読み込まれるまでは完了とみなさないでください。

## まとめと次のステップ

Space Quiz を公開し、エージェントの報告だけに頼らず、デプロイを確認しました。[レッスン 13: 振り返りと次のステップ][next-lesson]に進みます。

[next-lesson]: ../13-review/
