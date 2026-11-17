---
title: "Exercício 2 - Adicionar funcionalidades com o modo de agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até mesmo uma pequena atualização de aplicativo pode exigir alterações em vários arquivos e execução de testes. O modo de agente do Copilot pode explorar a estrutura do projeto, identificar os arquivos relevantes, coordenar alterações, executar tarefas como testes e corrigir problemas encontrados. Neste exercício, use-o para adicionar uma funcionalidade ao site.

> [!NOTE]
> Embora tenham nomes semelhantes, o modo de agente e o agente de nuvem oferecem experiências diferentes. O modo de agente trabalha no ambiente de desenvolvimento integrado (IDE), com ciclos rápidos de feedback e interação. O agente de nuvem trabalha de forma assíncrona em issues e pull requests, como uma pessoa da equipe.

Neste exercício, você vai:

- usar o modo de agente para explorar o projeto, identificar arquivos e coordenar alterações.
- implementar uma funcionalidade na interface e na camada de dados.
- revisar as alterações e os testes antes do merge.

## Cenário

À medida que a lista de jogos cresce, as pessoas precisam filtrar por categoria e distribuidora. Você já criou um auxiliar de distribuidoras no exercício anterior; agora, concluirá as alterações restantes da camada de dados, da interface e dos testes.

## Executar o site Tailspin Toys

Antes de alterar o código, examine o funcionamento atual do site. A Tailspin Toys é uma plataforma de crowdfunding para jogos de tabuleiro com temática de desenvolvimento. O aplicativo Astro gera HTML estático durante o build. As páginas consultam diretamente um banco de dados SQLite local com Drizzle ORM; não há uma API de backend separada nem framework de interface no cliente. Os auxiliares de acesso a dados ficam em `src/lib/`, e a interatividade usa um elemento `<script>` Astro pequeno e escopado, com APIs padrão do DOM.

### Iniciar o site

O comando `npm run dev` inicia o servidor de desenvolvimento Astro.

1. Volte ao codespace. Você deve estar na branch `main` depois do exercício anterior.
2. Abra um terminal com <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie uma branch para a filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicie o site:

   ```bash
   npm run dev
   ```

   Quando o servidor Astro estiver pronto, será exibido um endereço semelhante a:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

5. Mantenha pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecione `http://localhost:4321` no terminal para abrir o site.

> [!NOTE]
> No Codespaces, o link localhost abre um túnel privado como `https://<your-codespace-name>-4321.app.github.dev/`.

Se uma caixa de diálogo perguntar se deseja abrir uma janela do navegador para `http://localhost:4321`, feche-a selecionando **x**.

### Explorar o site

Os principais recursos do site incluem:

- **Página inicial**: exibe uma lista de jogos de tabuleiro com títulos, imagens e descrições.
- **Página de detalhes do jogo**: ao selecionar um jogo, você abre uma página com mais informações, incluindo título, descrição, distribuidora e categoria.

## Explorar o backlog com o Copilot

Antes de começar, abra o painel Copilot Chat e selecione **New Chat** para criar uma conversa limpa. O modo e o modelo variam por exercício.

1. Na lista de agentes da visualização Chat, selecione **Agent**. O agente planeja e implementa alterações em vários arquivos de forma autônoma, executa comandos do terminal e invoca ferramentas.

   ![Seletor de agente na visualização Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecione **Claude Sonnet 4.5** na lista de modelos.

> [!CAUTION]
> A inclusão de **Claude Sonnet 4.5** reflete o modelo usado ao preparar este workshop, não uma preferência por um modelo específico. Como os modelos de linguagem são probabilísticos, as sugestões podem variar; isso é esperado.

3. Peça ao Copilot que examine o backlog:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecione **Continue** para executar o comando que lista as issues e revise a lista gerada. Observe como o Copilot prioriza os itens de acordo com o que considera mais útil para as pessoas usuárias.

> [!NOTE]
> Por causa do caráter probabilístico dos modelos de linguagem, o Copilot talvez use um comando do Model Context Protocol (MCP) diferente, mas ainda deverá concluir a tarefa.

## Revisar os arquivos de instruções

Antes de pedir ao agente que gere código, examine `.github/instructions/ui.instructions.md`, que explica como abordar alterações de funcionalidades no site.

1. No codespace, abra `.github/instructions/ui.instructions.md`.
2. Observe a arquitetura, os princípios de design de componentes, testabilidade e acessibilidade e os links para instruções de Astro, Tailwind CSS e Drizzle.

> [!TIP]
> Os arquivos de instruções podem referenciar outros arquivos de instruções e arquivos do projeto. Os caminhos são relativos ao arquivo de instruções, o que facilita reutilizar orientações e exemplos.

## Implementar a filtragem

Concluir a filtragem exige atualizar a lógica da camada de dados em `src/lib/`, os testes e a interface da página de listagem. Os testes devem ser aprovados antes do merge. Use o modo de agente para realizar esse trabalho.

1. Continue a conversa atual ou selecione **New Chat**.
2. Selecione **Add Context**, **Instructions** e **ui**.

   ![Exemplo de seleção do arquivo de instruções da interface](../../../_images/ex3-select-instructions-file.png)

3. Confirme que **Agent** está selecionado.
4. Confirme que **Claude Sonnet 4.5** continua selecionado.
5. Peça ao Copilot que implemente a funcionalidade associada à issue:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Acompanhe o Copilot enquanto explora o projeto, localiza os arquivos relacionados ao recurso, como os auxiliares da camada de dados, a interface e os testes, altera os arquivos e executa os testes.

   ![Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

7. Quando solicitado pelo Copilot, selecione **Continue** para executar os testes.

   ![Solicitação no painel Copilot Chat para confirmar a execução dos testes](../../../_images/ex3-agent-mode-run-tests.png)

8. É normal haver pausas e falhas em alguns testes durante o processo. O Copilot alterna entre gerar código e executar testes até concluir a tarefa sem detectar erros.

   ![Sessão concluída do Copilot Chat no modo de agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Examine o código gerado para identificar possíveis problemas.

> [!CAUTION]
> Sempre revise o código gerado pelo Copilot ou por outras ferramentas de IA.

10. Volte ao navegador com o site em execução e experimente a nova funcionalidade.
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela Copilot Chat.

> [!NOTE]
> Durante o trabalho, os botões **Keep** e **Undo** podem aparecer no editor. Aguarde o agente terminar; então, revise e aceite ou reverta as alterações.

## Resumo e próximos passos

Você usou o modo de agente do GitHub Copilot para adicionar funcionalidades ao site, explorar o projeto e coordenar alterações na interface e na camada de dados. Também revisou o código e os testes antes do merge.

Em seguida, você [testará o recurso com o servidor MCP do Playwright][next-lesson] e abrirá um pull request.

### Exercício bônus: implementar paginação

À medida que a lista de jogos cresce, será necessário implementar paginação. Usando o que aprendeu neste exercício, peça ao Copilot que atualize o site. Considere:

- seguir as práticas recomendadas existentes, inclusive usar os arquivos de instruções.
- decidir como implementar a paginação: permitir que a pessoa usuária escolha o tamanho da página ou defini-lo previamente.
- fornecer contexto suficiente no prompt para orientar a implementação.
- iterar com o GitHub Copilot, solicitando alterações e fornecendo contexto adicional. Esse é o fluxo normal ao trabalhar com o Copilot.

## Recursos

- [Copilot: modos ask, edit e agent][choose-mode]
- [Modo de agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
