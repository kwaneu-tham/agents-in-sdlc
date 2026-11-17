---
title: "演習 5: GitHub Copilot の作業を反復的に改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー体験と機能を改善する複数のタスクで GitHub Copilot を使いました。コードへのドキュメント追加、デザインチーム向けの関連ゲーム機能、ハイコントラストとライトモードの切り替えを含むアクセシビリティ機能を依頼しました。コードを確認し、必要に応じて Copilot に修正を依頼します。

### シナリオ

生成 AI を導入しても、ソフトウェア設計や DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに従います。チームにレビューを依頼する前に、Copilot が作成したドキュメント、関連ゲーム機能、アクセシビリティ機能を確認します。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は監督なしで非同期にタスクを実行するため、安全を保つための制約があります。

- Copilot はリポジトリへの読み取りアクセスを持ち、書き込みは作業用ブランチにのみ許可されます。
- Cloud agent は GitHub Actions 内の一時的な独立環境で実行されます。
- GitHub Actions ワークフローを実行する前に、人間による承認が必要です。
- [外部リソースへのアクセスは既定で制限][agent-firewall]されています。Model Context Protocol (MCP) サーバーへのアクセスも制限されます。

## 生成されたドキュメントをレビューする

まず、コードにドキュメントを追加するために GitHub Copilot cloud agent が作成した最初のプルリクエスト (PR) を、GitHub.com の標準的な PR 画面で確認します。

> [!NOTE]
> PR に GitHub Copilot がファイアウォールでブロックされたという警告が表示されることがあります。Copilot は外部 MCP サーバーへの呼び出しを含む外部リソースへのアクセスが既定で制限されているため、これは想定された動作です。必要であれば、[Copilot cloud agent のファイアウォールを設定または無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** などのタイトルの PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が付きます。その場合は完了するまで待ちます。数分かかることがあるため、その間に休憩したり、これまでに学んだことを振り返ったりしてください。

4. PR の準備ができたら **Files changed** タブを選択し、変更をレビューします。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく追加された TSDoc ドキュメントコメントなど、更新されたコードを確認します。具体的な変更内容は異なる場合があります。TSDoc コメント、TypeScript の規約、コメントヘッダーに注目してください。これらは演習 2 の最初に確認したカスタム指示ファイルから反映されています。
6. 更新内容に問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示を探します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄にワークフローがキューに入ったことを確認します。順調に進めば、単一 Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。

## GitHub Copilot に変更を依頼する

PR では、チームメンバーと同様にコメントやコード行へのインラインコメントで Copilot に変更を依頼できます。Copilot はコメントを確認し、対応する別のセッションを開始します。結果は確率的に変化するため、プロンプトを一律に指定することはできません。たとえば、次のような変更を依頼できます。

- 各コードファイルの先頭に、ファイルの役割を説明するコメントヘッダーを追加する。
- TypeScript と Astro ファイルに TSDoc ドキュメントコメントを追加する。
- Astro アプリの構造を説明する README を作成する。

1. 生成されたドキュメントへの変更を依頼するコメントを追加し、ほかのユーザーと同じように **@copilot** を指定します。上記の例や、コードベースに必要なドキュメントに関する別の依頼を使います。
2. **View Session** を選択して Copilot の作業を確認します。Copilot が更新用の新しいセッションを開始します。
3. **Back to pull request** を選択して PR に戻ります。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が終わったら、PR に新しいコミットが追加されたことを確認します。
5. **Files changed** タブを選択し、変更をレビューします。

満足するまで反復できます。準備ができたら、PR をドラフトからレビュー可能な状態に変更して `main` ブランチにマージします。

![Convert PR to ready](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue に対し、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルの PR を選択します。
4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 更新内容に問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. ワークフローの承認待ちを示す表示を探します。
7. ワークフローが承認待ちの場合は **Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. PR のチェック欄にワークフローがキューに入ったことを確認します。順調に進めば、単一 Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。
9. **任意:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動でテストできます。Codespace のターミナルを開き、Copilot が作成したブランチ名 (例: **copilot/fix-8**) に `<branch-name>` を置き換えて、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

関連ゲーム機能が実装されました。ほかの PR と同様に、Copilot cloud agent にコメントを追加して更新を依頼できます。たとえば表示する関連ゲームの数を調整したり、(カスタム指示を更新する前に割り当てたため) コメントヘッダーや TSDoc ドキュメントコメントを追加したりできます。**Conversation** タブにコメントを追加すると、Copilot が確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用カスタムエージェントで実装した機能を確認します。この PR には、演習 3 で依頼したハイコントラストモードと、演習 4 でコントロールセンターから依頼したライトモードの両方が含まれます。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルの PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が付きます。その場合は作業が完了するまで待ちます。数分かかることがあります。

4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 次の点に注意しながら実装を確認します。
   - モードを切り替えるユーザーインターフェイス (UI) コンポーネント。
   - 利用者の設定を保存する local storage の使い方。
   - ハイコントラストモードとライトモードの CSS またはスタイルの変更。
   - アクセシブルなリッチインターネットアプリケーション (ARIA) 属性 (ラベル、キーボード操作など)。
   - モード切り替えを管理する JavaScript/TypeScript コード。
6. 更新内容に問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示を探します。
8. ワークフローが承認待ちの場合は **Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄にワークフローがキューに入ったことを確認します。順調に進めば、単一 Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。
10. **任意:** Codespace でこのブランチに切り替えて、アクセシビリティ機能を手動でテストできます。Codespace のターミナルを開き、Copilot が作成したブランチ名に `<branch-name>` を置き換えて、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    アプリを起動し、ハイコントラストとライトモードの切り替えをブラウザーで試します。ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティエージェントによって、Copilot はアクセシビリティのベストプラクティスに沿って機能を実装しました。懸念や改善点があれば、前の PR と同様にコメントで **@copilot** に更新を依頼できます。

## 任意の演習 — 作業の委任を続ける

cloud agent に実際のバックログ項目を割り当て、その間に別の作業を進める方法を練習します。たとえば、次の Issue を作成して Copilot に割り当てます。

- ゲーム詳細ページに支援希望者向けのフォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセスヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。これらのレッスンでは、次の作業を行いました。

- このリポジトリに含まれるカスタム指示ファイルを確認し、cloud agent の出力に反映される内容を把握した。
- Copilot cloud agent に Issue を割り当て、環境の準備、計画、非同期の実行を確認した。
- アクセシビリティ用のカスタムエージェントを作成して使い、ハイコントラストとライトモードの切り替えを追加した。
- Copilot エージェントページをコントロールセンターとして使い、アクセシビリティセッションを監視して調整した。
- cloud agent の PR をレビューして反復し、**@copilot** に変更を依頼し、ワークフローを承認した。

## レビューと次のステップ

Cloud agent ハーネスを完了しました。さらに確認する場合は、ほかのハーネスで今回の内容を別の環境に応用できます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE 内で Copilot Chat のエージェントモードと MCP の統合を確認します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot の各モード、エージェントスキル、QA カスタムエージェント、Playwright MCP、Agent Merge、cloud agent への任意の `/delegate` を使い、ターミナルから2つの変更をレビューしてリリースします。

自分のリポジトリでは、次の作業も試せます。

- リファクタリングやテストカバレッジの Issue を cloud agent に割り当てる。
- 別の分野向けのカスタムエージェントを作成する。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
