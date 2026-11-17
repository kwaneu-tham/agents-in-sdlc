---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Instruções personalizadas][previous-lesson]

Quase todas as organizações enfrentam dívida técnica, como problemas de segurança não resolvidos, código legado que precisa de atualização ou solicitações de recursos que permanecem no backlog por falta de tempo. O agente de nuvem do GitHub Copilot pode atualizar código e adicionar funcionalidades de forma autônoma. Ao concluir o trabalho, ele cria um pull request (PR) de rascunho para revisão por uma pessoa desenvolvedora. Assim, a equipe pode delegar tarefas repetitivas e se concentrar em atividades de maior impacto.

Neste exercício, você explorará como:

- personalizar o ambiente de geração de código.
- garantir que as operações sejam executadas com segurança.
- delimitar issues com clareza.
- atribuir issues ao Copilot.

## Cenários

A Tailspin Toys quer reduzir a dívida técnica. A documentação deixada pelas pessoas contratadas para criar a primeira versão do site está em estado inadequado: ela está completamente ausente. Como primeiro passo, a equipe quer comentários TSDoc em todas as funções exportadas do aplicativo. Além disso, a equipe de design quer mostrar jogos relacionados — outros títulos da mesma categoria — na página de detalhes de cada jogo para explorar a experiência e fazer testes de aceitação. A equipe ainda não precisa de uma implementação refinada; quer algo que possa usar para avaliar a experiência. Esse trabalho é um bloqueio, mas há outras issues de prioridade maior. Como essas tarefas podem perder prioridade, são boas candidatas à execução assíncrona pelo agente de nuvem.

## Apresentação do agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot][cloud-agent-overview] executa tarefas em segundo plano de forma semelhante a uma pessoa desenvolvedora. Você pode, por exemplo, [atribuir uma issue do GitHub ao Copilot][assign-issue]. Depois da atribuição, o Copilot cria um pull request de rascunho para acompanhar o progresso, configura um ambiente e começa a trabalhar. Você pode examinar a sessão enquanto o trabalho está em andamento ou depois de concluído. Quando a solução estiver pronta para revisão, o Copilot marcará você no pull request.

## A importância de instruções bem delimitadas

O GitHub Copilot não elimina a necessidade de pensar. Mesmo tarefas aparentemente simples podem ter detalhes complexos. [Delimite cuidadosamente as tarefas para o agente de nuvem][cloud-agent-best-practices]. Trate-o como uma pessoa parceira de programação com IA: trabalhe por etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software continuam os mesmos.

## Instruções personalizadas neste repositório

Antes de atribuir trabalho, examine os arquivos de instruções incluídos no repositório. Abra-os na interface Web do GitHub ou no codespace:

- `.github/copilot-instructions.md`: examine **Code standards**, especialmente as convenções de TypeScript e os comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observe o frontmatter `applyTo`, que aplica as instruções a arquivos `**/*.test.ts`.

Ao atribuir a issue `Code lacks documentation`, observe no pull request os comentários TSDoc, as convenções de TypeScript e os cabeçalhos de arquivo definidos nessas instruções.

## Configurar o ambiente de desenvolvimento do agente de nuvem

O agente de nuvem usa o [GitHub Actions][github-actions] como ambiente de trabalho. Você pode personalizá-lo com um workflow de configuração em `.github/workflows/copilot-setup-steps.yml`. O projeto já inclui esse workflow, que configura Node.js, dependências, navegadores para testes de ponta a ponta e o banco de dados SQLite migrado e preenchido para o aplicativo Astro:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões mínimas necessárias para estas etapas. O Copilot recebe seu próprio token para as operações dele.
    permissions:
      # Para clonar o repositório nas etapas de configuração, por exemplo, para instalar dependências, use `contents: read`. Se não clonar o repositório, o Copilot fará isso automaticamente depois que as etapas forem concluídas.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do aplicativo: Node.js (o aplicativo agora usa Astro, Drizzle e libSQL)
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

      # Migra e preenche o banco de dados SQLite local para que builds e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

O workflow tem um único job, `copilot-setup-steps`, executado antes de o Copilot começar a trabalhar no pull request. O gatilho `workflow_dispatch` permite executar manualmente o workflow na guia **Actions** para testar a configuração.

## Adicionar documentação

A falta de documentação pode reduzir a produtividade e dificultar a manutenção ou a integração de novas pessoas à equipe. Atribua ao agente de nuvem uma issue para documentar o código; você revisará o resultado em um exercício futuro.

1. Em uma nova guia do navegador, acesse o repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue**.
5. No campo **Title**, digite `Code lacks documentation`.
6. No campo **Description**, insira:

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

   ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

   ![Detalhes da atribuição do Copilot](../../../_images/ex4-assign-copilot-details.png)

9. Selecione **Assign**.
10. Selecione a guia **Pull Requests**.
11. Abra o pull request (PR) recém-criado, com título semelhante a `[WIP]: Code lacks documentation`. Se ele ainda não aparecer na lista, aguarde alguns instantes e atualize a página.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas pode levar alguns minutos para aparecer no PR. O Copilot está configurando o ambiente com o workflow descrito anteriormente, analisando o projeto e definindo uma abordagem.

13. Revise a lista e as tarefas que o Copilot executará.
14. Role a linha do tempo do pull request e procure uma atualização informando que o Copilot começou a trabalhar na issue.
15. Selecione **View session**.

   ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Role a sessão ao vivo e observe como o Copilot explora o código, entende o estado do projeto, pausa para pensar e decidir uma abordagem e cria código.

Enquanto o Copilot trabalha nessa tarefa, atribua outra issue para aproveitar a execução assíncrona.

## Adicionar jogos relacionados à página de detalhes

1. Volte ao repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para usar o modelo em branco.
5. No campo **Title**, digite `Show related games on the game details page`.
6. No campo **Description**, insira:

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

   ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Pouco depois, um emoji de olhos aparecerá no primeiro comentário da issue, indicando que o Copilot começou a trabalhar.

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

O Copilot está trabalhando na segunda solicitação. Como o agente de nuvem funciona de maneira semelhante a uma pessoa engenheira de software, não é necessário acompanhá-lo continuamente; revise o resultado quando o trabalho for concluído. Enquanto isso, continue para a criação e o uso de agentes personalizados.

## Resumo e próximos passos

Você explorou o [agente de nuvem do GitHub Copilot][cloud-agent] e atribuiu issues para execução assíncrona. O agente pode ajudar a reduzir dívida técnica, criar recursos e migrar código entre frameworks. Você também explorou a configuração do ambiente, a execução segura de operações e a importância de delimitar as issues.

Em seguida, você criará e usará agentes personalizados. O [agente de nuvem também pode usar servidores do Model Context Protocol (MCP)][cloud-agent-mcp] e as instruções personalizadas examinadas anteriormente.

## Recursos

- [Sobre o agente de nuvem do Copilot][cloud-agent]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente de nuvem][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
