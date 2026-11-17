---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使う際は、コンテキストが重要です。タスクの実行方法や Copilot が知っておくべき背景情報がある場合は、必要なコンテキストにアクセスできるようにします。[指示ファイル][instruction-files]を使うと、Copilot に「何を」するかだけでなく、「どのように」実行するかを伝えられます。

この演習では、次の内容を学習します。

- リポジトリのカスタム指示とパスに応じた指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる方法を確認する。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加する。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、この演習では指示の更新前後に同じプロンプトを実行しません。Copilot cloud agent は GitHub Issue に対して非同期で動作するため、リアルタイムで並べて効果を示すのが難しいためです。このハーネスの後半で cloud agent が作成した PR をレビューするときに、指示ファイルの影響を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には開発プラクティスのガイドラインがあります。

- データレイヤーには常にユニットテストを用意します。
- ユーザーインターフェイス (UI) はダークモードを使い、現代的な見た目にします。
- コードには TSDoc ドキュメントコメントを追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使い、Copilot がこれらのプラクティスに沿って作業できるようにします。

### カスタム指示

カスタム指示は、コーディングスタイルや好みを Copilot に伝え、より適切な提案やコードを得るための機能です。リポジトリ全体、または特定の種類のファイルやタスクに指示を作成できます。

- `.github/copilot-instructions.md` は、リポジトリに関するすべてのリクエストで Copilot に送られる指示ファイルです。技術スタック、開発中の内容、ベストプラクティスなど、プロジェクト全体に関係する情報を記載します。
- `.github/instructions/*.instructions.md` は、TypeScript や Astro などの言語や、UI コンポーネントまたはユニットテストの作成など特定のタスクに対するガイダンスを提供します。

> [!NOTE]
> 統合開発環境 (IDE) では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には適用されません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルの指示と `applyTo` フロントマターがある `*.instructions.md` ファイルを使ってコードを生成します。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準の指示ファイルにも対応しています。詳細は[カスタム指示の対応状況][custom-instructions-support]を確認してください。

### 指示ファイルを管理するためのベストプラクティス

このプロジェクトの例には、指示ファイルの代表的な構成方法が示されています。

- `copilot-instructions.md` には、構築するもの、プロジェクトの構造、全体的なコーディング標準など、プロジェクトレベルの情報を記載します。
- `*.instructions.md` ファイルには、ユニットテスト、Astro コンポーネント、データレイヤーなどのファイル形式や特定のタスク向けの指示を記載します。
- 自然な言葉を使い、明確なガイダンスと、コードの望ましい例・望ましくない例を示します。

指示ファイルの作成方法や AI の使い方に唯一の正解はありません。プロジェクトで試しながら、適切な方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な指示ファイルを用意することをお勧めします。このプロジェクトには [UI の更新][ui-instructions]や [Astro][astro-instructions] などの指示があります。
>
> Copilot に指示ファイルを作成させることもできます。環境によって操作は異なります (VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init` など)。必要な場合は、使用中の環境向けのレッスンで説明します。
>
> テンプレートや出発点を探す場合は、指示ファイル、カスタムエージェントなどを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

リポジトリの指示ファイルをエディターまたは GitHub の Web UI で開き、既存のガイダンスを確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の **GitHub Actions Workflows** の指示は、Copilot とのすべてのやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、Astro ファイル、Drizzle データレイヤー、テストなどの指示を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` がリポジトリルートを基準とする glob を定義していることを確認します。たとえば `**/*.test.ts` に一致する TypeScript のテストファイルに適用されます。
5. このプロジェクトでユニットテストを作成するときの指示を確認します。
6. `.github/instructions/drizzle.instructions.md` の末尾を開き、`unit-tests.instructions.md` などの指示ファイルやプロジェクト内の既存ファイルへのリンクを確認します。パスはリポジトリルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関する要件はまだありません。次に TSDoc ドキュメントコメントとファイル先頭のコメントヘッダーに関するルールを追加します。

## リポジトリ標準を追加する

この演習では、コード内のドキュメントを TSDoc コメントとファイルレベルのコメントヘッダーで記述するルールを追加します。cloud agent が後の演習で Issue に取り組むときに、この指示を読み込みます。

まず、変更用のブランチを作成します。cloud agent は対象 Issue のブランチから指示を読み込みますが、ここでは変更を個別にコミットします。

1. Codespace のターミナルから新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` にプロジェクト全体の情報を記載し、コーディング標準を Copilot に伝えます。

1. `.github/copilot-instructions.md` を再度開きます。
2. 27行目付近にある **Code formatting requirements** セクションを探します。現在はコーディング標準が記載されていますが、コード内ドキュメントのルールがありません。
3. 既存の標準のすぐ下に次の Markdown を追加し、ファイルヘッダーと TSDoc ドキュメントコメントを指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 指示ファイルはリポジトリ全体に適用する `.github/copilot-instructions.md` と、言語、ファイル形式、タスクに適用する `*.instructions.md` のどちらにも作成できます。コードコメントのルールのようにプロジェクト全体に関係する標準は、リポジトリレベルのファイルに適しています。

## 指示の更新をコミット、プッシュ、マージする

次の演習で Issue を Copilot に割り当てると、Copilot は `main` からブランチを作成します。指示を読み込ませるには、更新を `main` にマージする必要があります。

1. ファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを開き、マージします。次の演習で Issue に取り組むとき、cloud agent は更新された指示を読み込みます。

> [!TIP]
> このワークショップで `main` に直接作業する場合は、ブランチの作成とプッシュを省略して、`main` に直接コミットできます。ここでは実際のプロジェクトでの手順に近づけるため、ブランチを使っています。

## まとめと次のステップ

このプロジェクトの指示ファイルから Copilot がコンテキストを取得する方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。このハーネスの後半で cloud agent が作成する PR をレビューするときに、この標準が適用されているか確認します。

次は、[最初の Issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を書くための5つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
