---
title: "Exercício 3 - Testar o recurso com o servidor MCP do Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Modo de agente][previous-lesson] · [Próxima lição: Agentes personalizados →][next-lesson]

Você acabou de criar a funcionalidade de filtragem com o modo de agente. Antes de abrir um pull request, confirme que ela funciona no navegador. Configure o servidor do Model Context Protocol (MCP) do Playwright para que o Copilot controle um navegador real, teste o recurso e, em seguida, publique a branch e abra o pull request (PR).

Neste exercício, você vai:

- conhecer o Model Context Protocol (MCP) e como servidores MCP ampliam as ferramentas do Copilot.
- adicionar o servidor MCP do Playwright ao espaço de trabalho.
- pedir ao Copilot que teste manualmente a filtragem no navegador.
- publicar a branch e abrir um pull request.

## O que é o Model Context Protocol (MCP)?

O [Model Context Protocol (MCP)][mcp-blog-post] é um padrão que permite aos agentes de IA se comunicar com ferramentas e serviços externos em tempo real. Eles podem acessar informações atualizadas usando recursos e executar ações em seu nome usando ferramentas. Um servidor MCP funciona como ponte entre o agente e as ferramentas e serviços externos, gerenciando a comunicação, por exemplo, com APIs existentes ou ferramentas locais como pacotes npm. Cada servidor disponibiliza um conjunto diferente de ferramentas e recursos. O [servidor MCP do GitHub][github-mcp-server] fornece APIs para gerenciar repositórios, issues e pull requests; o [servidor MCP do Playwright][playwright-mcp-server] permite automatizar um navegador. O GitHub mantém um [registro de MCP][mcp-registry] para facilitar a descoberta de servidores e as contribuições ao ecossistema.

![Diagrama da interação do modo de agente com contexto, modelo de linguagem de grande porte (LLM) e ferramentas, incluindo ferramentas fornecidas por servidores MCP e extensões do VS Code](../../../_images/ex1-mcp-diagram.png)

> [!CAUTION]
> Trate servidores MCP como qualquer outra dependência. Revise o código-fonte, verifique o publicador e considere as implicações de segurança. Use apenas servidores confiáveis e tenha cuidado ao conceder acesso a recursos ou operações sensíveis.

## Revisar a configuração do MCP

O arquivo `.vscode/mcp.json` configura os servidores MCP disponíveis neste espaço de trabalho.

1. Abra `.vscode/mcp.json` no codespace.
2. Confirme que o servidor `github` já está configurado:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

O modelo do projeto inclui essa entrada e usa o [servidor MCP remoto do GitHub][remote-github-mcp-server]. Não é preciso instalá-lo localmente: o VS Code se conecta por HTTP e solicita autenticação do GitHub na primeira utilização.

## Adicionar o servidor MCP do Playwright

O servidor MCP do Playwright dá ao Copilot um navegador controlável para testar o recurso.

1. Em `.vscode/mcp.json`, adicione `playwright` ao lado de `github`:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. Salve o arquivo.

Diferentemente do servidor remoto `github`, `playwright` é um servidor **local**: o VS Code o inicia no computador executando `npx @playwright/mcp@latest`. A opção `--headless` executa o navegador sem uma janela visível, necessária no codespace, que não tem uma área de trabalho para exibi-lo. O projeto Tailspin Toys já usa o Playwright nos testes de ponta a ponta, então o navegador necessário normalmente já está instalado. Se o Copilot informar que falta um navegador, peça que execute `npx playwright install chromium` e tente novamente.

## Iniciar e confiar no servidor

O VS Code inicia servidores MCP sob demanda, quando o Copilot precisa das ferramentas.

1. Em `.vscode/mcp.json`, selecione **Start** acima da entrada `playwright` para iniciar o servidor e confirmar a configuração.
2. Se o VS Code perguntar se você confia no servidor, revise a configuração e confirme para iniciá-lo.

Iniciar o servidor manualmente é opcional. Se você não fizer isso, o VS Code o iniciará quando o Copilot usar as ferramentas e solicitará que você confirme a confiança.

## Testar a filtragem

O servidor MCP do Playwright fornece ao Copilot um navegador real. Em vez de você navegar pelo aplicativo para verificar o trabalho, o agente pode abrir uma página, navegar, aplicar filtros, ler os resultados e resumir o que observou. É uma maneira rápida de confirmar o comportamento sem sair da conversa.

Internamente, o servidor MCP do Playwright usa a [árvore de acessibilidade][playwright-mcp-server] da página, em vez de capturas de tela. Assim, o agente raciocina sobre elementos estruturados e rotulados, como botões, links e itens de lista, da mesma forma que uma tecnologia assistiva. Portanto, uma verificação funcional rápida também serve como uma verificação básica de acessibilidade.

Com o servidor conectado e o aplicativo em execução, peça ao Copilot que teste o recurso:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

O Copilot abrirá um navegador pelo servidor MCP do Playwright, seguirá as etapas e relatará o que observou. Compare o relatório com os critérios de aceitação da issue. Se algo estiver incorreto, faça perguntas ou peça que o Copilot corrija o código antes de abrir o PR.

> [!NOTE]
> O aplicativo precisa estar em execução em `http://localhost:4321`. Se o servidor de desenvolvimento tiver sido interrompido, inicie-o novamente. Na primeira utilização, o Playwright talvez precise baixar um navegador; se isso acontecer, peça ao Copilot que execute `npx playwright install chromium`.

## Publicar a branch e criar um pull request

1. Abra o painel **Source Control** no codespace e revise as alterações feitas pelo Copilot.
2. Selecione **+** para preparar as alterações.
3. Use o botão **Sparkle** para gerar uma mensagem de commit.

   ![Painel Source Control com as alterações](../../../_images/ex3-source-control-changes.png)

4. Selecione **Publish** para enviar a branch ao repositório.

## Criar o pull request

Peça ao Copilot que encontre a issue relacionada e crie um PR associado a ela.

1. No painel Copilot Chat, selecione **New Chat**.
2. Confirme que **Agent** está selecionado para permitir o uso das ferramentas do GitHub.
3. Envie o prompt:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Quando necessário, selecione **Continue** para permitir as ações. Na primeira utilização de uma ferramenta do GitHub, talvez seja solicitado que você se autentique.
5. Confirme que o Copilot encontrou a issue correta e criou o PR.
6. Selecione o link para revisar o PR, mas **não faça merge ainda**.

## Resumo e próximos passos

Você conheceu o MCP, adicionou o servidor do Playwright, testou a filtragem no navegador e publicou a branch para abrir um pull request. Em seguida, você [criará um agente personalizado][next-lesson] para facilitar tarefas específicas, como revisões de acessibilidade.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
- [Servidor MCP do Microsoft Playwright][playwright-mcp-server]
- [Servidor MCP do GitHub][github-mcp-server]
- [Registro de MCP do GitHub][mcp-registry]
- [Servidores MCP no VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
