---
title: "演習 2 - Copilot エージェントモードで機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

簡単なアプリケーションの更新でも、複数のファイルを変更し、テストを実行する必要があることがよくあります。必要なファイルを探し、変更し、テストし、問題をデバッグして修正する作業を繰り返します。

Copilot のエージェントモードを使うと、IDE 内でより自律的に作業できます。エージェントはプロジェクトの構造を調べ、必要なファイルを変更し、テストなどのタスクを実行して、見つかった問題を修正します。サイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、エージェントモードと cloud agent は異なる用途です。エージェントモードは IDE 内で動作し、短いフィードバックサイクルで対話できます。Cloud agent はチームメンバーのように非同期で作業し、Issue とプルリクエストを通じてやり取りします。

この演習では、次の内容を学習します。

- Copilot エージェントモードでプロジェクトを調べ、関連ファイルを特定し、複数ファイルを連携して変更する。
- UI とデータレイヤーにまたがる新機能を実装する。
- コードベースにマージする前に、Copilot が生成した変更とテストをレビューする。

## シナリオ

ゲーム一覧が増えてきたため、カテゴリーとパブリッシャーで絞り込めるようにします。前の演習で publishers ヘルパーを追加しました。ここでは Copilot エージェントモードを使い、残りのデータレイヤー、ユーザーインターフェイス (UI)、テストを完成させます。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、現在の機能を確認します。

Tailspin Toys は、開発者をテーマにしたボードゲームのクラウドファンディングプラットフォームです。ゲーム一覧と詳細を表示します。サイトは Astro アプリ1つで構成され、ビルド時に静的 HTML を生成します。ページは frontmatter 内で Drizzle オブジェクトリレーショナルマッパー (ORM) を使ってローカル SQLite データベースを直接検索します。別のバックエンドアプリケーションプログラミングインターフェイス (API) やクライアント側 UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` にあり、インタラクティブな動作には標準の Document Object Model (DOM) API を使う小規模な Astro `<script>` を使用します。

### Web サイトを起動する

`npm run dev` タスクで単一の Astro 開発サーバーを起動します。

1. Codespace に戻ります。前の演習から `main` に戻っていることを確認します。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して Codespace 内で新しいターミナルを開きます。
3. フィルター作業用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドで Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のような URL が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くブラウザーウィンドウが表示された場合は、**x** を選択して閉じます。

5. ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を、Mac では <kbd>Command</kbd>、Windows/Linux では <kbd>Ctrl</kbd> を押しながら選択して Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost の URL を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動的に転送されます。これは Codespace の Web サーバーに接続するプライベートトンネルです。

### Web サイトを確認する

サイトを起動したら、現在の機能を確認します。

- **Home Page**: タイトル、画像、説明を含むボードゲーム一覧を表示します。
- **Game Details Page**: ゲームを選択すると、タイトル、説明、パブリッシャー、カテゴリーなどの詳細ページを表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

Web サイトの初期実装は動作しています。テンプレートからプロジェクトを作成すると、Issue のバックログも自動的に作成されます。Copilot に項目を表示するよう依頼します。

1. Chat ビューのエージェントドロップダウンから **Agent** を選択します。エージェントモードは複数ファイルを調べて変更し、ターミナルコマンドを実行してツールを呼び出します。

   ![Chat ビューのエージェントピッカーを示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップは特定のモデルを推奨するものではありません。作成時に Claude Sonnet 4.5 を使用したため、手順に含めています。出力を比較的一貫させることを意図していますが、大規模言語モデル (LLM) は確率的に動作するため、提案は異なる場合があります。これは正常な動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot が異なる MCP コマンドを使う場合がありますが、タスクは完了できるはずです。

3. 次のプロンプトを Copilot に送り、バックログを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択し、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot が利用者に最も役立つと考える項目を優先順位付けしたことを確認します。

## 指示ファイルを確認する

コード生成を依頼する前に、Copilot にコンテキストを提供する指示ファイルを確認します。Web サイトへの機能追加方法が記載された [user interface (UI) 指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加に関するガイダンスを確認します。
   - アーキテクチャの概要。
   - コンポーネントの設計、テスト容易性、アクセシビリティの原則。
   - Astro、Tailwind CSS、Drizzle などのファイル形式に応じた指示へのリンク。

> [!TIP]
> 指示ファイルから、ほかの指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルの場所からの相対パスです。指示を再利用し、大きな説明を小さく分け、コード生成の例やテンプレートを示せます。

## フィルター機能を実装する

フィルター機能を完成させるには、少なくとも次の3種類の変更が必要です。

- データレイヤー (`src/lib/`) の残りのフィルターロジックを追加または調整する。
- フィルターの動作をテストするテストを追加または更新する。
- ゲーム一覧ページにフィルター UI を追加する。

コードベースにマージする前にテストが成功する必要があります。Copilot エージェントモードにこれらの作業を依頼します。

1. 現在の会話を続けるか、**New Chat** を選択して新しい会話を開始します。
2. **Add Context**、**Instructions**、**ui** の順に選択し、指示ファイルを追加します。

   ![UI 指示ファイルの選択例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェントドロップダウンで **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェントピッカーを示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

4. モデルとして **Claude Sonnet 4.5** が選択されていることを確認します。
5. バックログ内の関連 Issue に基づいて機能を実装するよう、次のプロンプトを送信します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、機能に関連するファイルを特定する様子を確認します。データレイヤーのヘルパー、UI、テストを見つけ、ファイルを更新してテストを実行します。

   ![Copilot がプロジェクトファイルを調べる様子を示すスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトを調べ、ファイルを変更し、テストを実行します。タスクの複雑さやコードベースによって数分かかることがあります。作業中、コードエディターに **Keep** と **Undo** が表示される場合があります。作業中に選択する必要はなく、完了すると各変更に **Keep** または **Undo** が表示されます。

7. Copilot に求められたら、**Continue** を選択してテストを実行します。

   ![Copilot Chat でテスト実行の確認を求めるダイアログを示すスクリーンショット](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりすることがあります。Copilot はコード生成とテストを繰り返し、エラーを検出しなくなるまで作業します。

   ![Copilot エージェントモードの完了したチャットセッションを示すスクリーンショット](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot やほかの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いているブラウザーに戻り、新しい機能を試します。
11. 動作とコードの確認が終わったら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot エージェントモードで Tailspin Toys に機能を追加する方法を確認しました。具体的には、次の内容を学びました。

- UI とデータレイヤーにまたがる新機能を実装する。
- プロジェクトを調べ、関連ファイルを特定し、複数ファイルを連携して変更する。
- コードベースにマージする前に、Copilot が生成した変更とテストをレビューする。

次は、[Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### 追加の演習 - ページネーションを実装する

ゲーム一覧が増えるとページネーションが必要になります。この演習で学んだ方法を使い、Copilot にページネーションを追加するよう依頼します。次の点を検討してください。

- 既存の指示ファイルを使うなど、現在のベストプラクティスに従う。
- ページサイズを利用者が選択できるようにするか、固定にするか決める。
- 希望する実装に必要なガイダンスを、プロンプトに含める。
- 必要に応じて Copilot に追加のコンテキストを伝え、変更を依頼する。Copilot を使うときの通常の進め方です。

## リソース

- [Copilot の ask、edit、agent モード: 機能と使い分け][choose-mode]
- [VS Code のエージェントモード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
