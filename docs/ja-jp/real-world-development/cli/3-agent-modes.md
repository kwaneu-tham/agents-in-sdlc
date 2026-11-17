---
title: "レッスン 3 - エージェントモード: Plan と Autopilot"
description: "Plan で方針を合意し、Autopilot で Issue に基づくフィルター機能を構築し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初に小さな機能をプロジェクトに追加しました。より大きな変更には、さらに確かなプロセスが必要です。GitHub Copilot CLI は組織の既存の開発フローに沿って作業できます。このレッスンから、Issue を起点に機能を作成し、コードの妥当性と動作を確認して、プロジェクトにマージするまでのエージェント主導の開発プロセスを進めます。

> [!NOTE]
> 機能の作業が続く間は、同じ会話とブランチを使います。通常はファイルの種類ごとにブランチやプルリクエスト (PR) を分けますが、ここでは主要な概念に集中するため簡略化します。

このレッスンでは、次の内容を学習します。

- GitHub Issue から Copilot との新しい会話を開始する。
- Plan モードで要件を定義する。
- Autopilot モードで機能を実装する。
- コードをレビューする。
- 転送されたブラウザーで機能を手動で検証する。

この機能の作業を続けながら、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、Model Context Protocol (MCP) による検証を追加し、品質保証 (QA) エージェントを作成して、機能の PR を開きます。

## シナリオ

Tailspin Toys のカタログが増えているため、訪問者がカテゴリーとパブリッシャーでゲームを絞り込めるようにします。バックログの Issue には機能が説明されていますが、カテゴリーの組み合わせなど、実装前に合意が必要な詳細があります。Plan モードで決定し、Autopilot には範囲を定めた実装を許可します。

## 背景

AI コーディングエージェントを開発フローに導入しても、基本は変わりません。一般的な開発フローは次のとおりです。

1. 必要な作業を説明する Issue から始める。
2. 実装する内容を計画する。
3. コードを作成してレビューする。
4. テストを実行してコードを検証する。
5. 新機能を手動で検証する。
6. プルリクエスト (PR) を作成する。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージする。

> [!NOTE]
> チームや組織によって詳細は異なりますが、ほとんどの開発フローはこの手順に沿っています。

この標準的な進め方に従うと、AI が生成したコードが要件を満たし、手作業で書いたコードと同じ検証を受けるようにできます。

## 会話モード

**会話モード**は、エージェントに許可する自律性の程度を制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すとモードを切り替えられます。

- **Interactive**: 開発者とエージェントが協力して作業します。エージェントは変更を提案し、次の作業の前に入力を待ちます。
- **Plan**: エージェントは最初に計画を作成します。プロジェクトファイルの編集はできません。
- **Autopilot**: エージェントは自律的にコードを書き、テストを実行し、タスクが完了するまで反復します。

まず Plan モードで計画を確認し、その後 Autopilot で実装します。

## Issue から開始する

フィルター機能に着手する前に、Codespace とリポジトリを準備します。

1. Codespace に戻ります。停止している場合は再起動します。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新して、フィルター作業用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を2回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの Issue を選択します。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7` (または同様の番号) が表示されます。`#` を使うと、GitHub 上の Issue やプルリクエストを会話のコンテキストに追加できます。

## フィルター機能を計画する

計画を立てることで、Copilot に実装を依頼する前に機能やタスクの進め方を決められます。複雑な作業では、事前に計画を確認することをお勧めします。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下に **Plan** と表示されることを確認します。
2. 前の手順で追加した Issue 参照の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べ、適切な実装方法を判断して計画を作成します。

3. フィルターの動作について質問された場合は、希望に応じて回答します。唯一の正解はありません。
4. 計画ができたら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画を上から下まで確認します。
6. 判断と合わない部分があれば、Copilot に計画の修正を依頼します。

## Autopilot を承認する

計画を確認したら、Autopilot で実装します。Autopilot は、タスクが完了したと判断するまで Copilot が作業を反復できるようにします。

1. **Accept plan and build on autopilot (recommended)**、またはインストール済みバージョンに表示される同様の選択肢を選択します。
2. プロンプトの下に **Autopilot** と表示されていることを確認します。
3. Copilot が計画に沿ってコードを生成し、テストを実行する様子を確認します。

> [!NOTE]
> 承認するとすぐに実装が始まることがあります。必ず先に計画を確認してください。依存関係が不足している場合やポートが競合している場合は、チェック完了とみなす前に環境の問題を解決します。

## 実装をレビューして検証する

生成されたコードは、他のコードと同様にマージ前にレビューします。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示が **Plan** または **Autopilot** ではないことを確認します。
2. `/diff` を入力し、フィルターの実装とテストを確認します。
3. 結果を Issue と計画時に決めた内容と照らし合わせます。
4. レビューが終わったら <kbd>Esc</kbd> を押して差分ビューを閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解決を依頼します。

## 新機能を試す

Codespaces の転送ポートを使ってサイトを起動し、実際に動作することを確認します。

1. 次のプロンプトでアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら、**Open in Browser** を選択します。
3. 計画で合意したカテゴリーとパブリッシャーのフィルター、およびその組み合わせを試します。
4. リセットと結果なしの場合の動作が、Issue と合意した内容に一致することを確認します。
5. Codespace に戻り、Copilot に起動した開発サーバーの停止を依頼します。

## まとめと次のステップ

複数の会話モードを使って機能を構築し、レビューしました。具体的には、次の作業を行いました。

- GitHub Issue から Copilot との会話を開始した。
- Plan モードで要件を定義した。
- Autopilot モードで機能を実装した。
- コードをレビューした。
- 転送されたブラウザーで機能を手動検証した。

次は、[カスタム指示を使って][next-lesson]、生成されたコードが文書化された規約に従うようにします。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンドリファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
