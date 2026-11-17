---
title: "Exercício 1 - Instruções personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Modo de agente →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Os [arquivos de instruções][instruction-files] fornecem ao Copilot contexto sobre a geração de código e ajudam a comunicar não apenas *o que* você quer, mas também *como* o trabalho deve ser feito.

Neste exercício, você vai:

- explorar como o contexto do projeto, as diretrizes de código e os padrões de documentação chegam ao Copilot por meio de instruções do repositório e arquivos definidos por caminho.
- gerar a primeira parte dos dados para a filtragem (um auxiliar de distribuidoras) com as instruções atuais.
- adicionar um padrão geral a `.github/copilot-instructions.md`.
- repetir o mesmo prompt e observar o efeito da nova regra.
- fazer commit das instruções e do auxiliar em `main` para que o Copilot use as orientações atualizadas no próximo exercício.

> [!CAUTION]
> O código gerado pode não seguir todos os padrões definidos, pois o Copilot é não determinístico. O objetivo é observar a tendência após atualizar as instruções, não obter resultados idênticos caractere por caractere.

## Arquivos de instruções

### Cenário

A Tailspin Toys define estes requisitos de desenvolvimento:

- A camada de dados sempre precisa de testes unitários.
- A interface deve usar o modo escuro e ter uma aparência moderna.
- O código deve incluir comentários de documentação TSDoc.
- O início de cada arquivo deve conter comentários que descrevam sua finalidade.

Use arquivos de instruções para fornecer ao Copilot o contexto necessário para seguir essas práticas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot. Você pode indicar convenções, bibliotecas e comentários desejados, tanto para todo o repositório quanto para tipos específicos de arquivos ou tarefas.

Há dois tipos de arquivos:

- `.github/copilot-instructions.md`: um arquivo enviado ao Copilot em **todas** as solicitações do repositório. Ele deve conter informações gerais do projeto, como pilha tecnológica, visão geral, práticas recomendadas e orientações globais.
- `.github/instructions/*.instructions.md`: arquivos com diretrizes para linguagens, tipos de arquivo ou tarefas específicas, como TypeScript, Astro, componentes de interface e testes.

> [!NOTE]
> No ambiente de desenvolvimento integrado (IDE), os arquivos de instruções orientam a geração de código no Copilot Chat, não as sugestões de conclusão de código nem as próximas edições.
>
> Copilot Chat, Copilot CLI e agente de nuvem do Copilot usam as instruções do repositório e arquivos `*.instructions.md` com frontmatter `applyTo` ao gerar código.
>
> O Copilot também [aceita arquivos de instruções em outros padrões][custom-instructions-support], como AGENTS.md e CLAUDE.md.

### Práticas recomendadas para gerenciar arquivos de instruções

- Mantenha `copilot-instructions.md` focado em contexto do projeto, estrutura e padrões gerais.
- Use arquivos `*.instructions.md` para tipos de arquivo e tarefas específicas.
- Use linguagem natural e orientações claras, com exemplos do que fazer e evitar.

Não há uma única maneira correta de criar instruções ou usar IA. Experimente para descobrir o que funciona melhor.

> [!TIP]
> O projeto inclui instruções para várias tarefas, entre elas [atualizações de interface][ui-instructions] e [Astro][astro-instructions]. O Copilot também pode ajudar a gerar arquivos de instruções; por exemplo, use **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI. Para encontrar modelos, explore o [awesome-copilot][awesome-copilot].

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar os arquivos de instruções do projeto

1. Abra `.github/copilot-instructions.md`.
2. Examine a descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe as orientações aninhadas sobre **GitHub Actions Workflows**.
3. Examine a pasta `.github/instructions`, que contém orientações para Astro, a camada de dados Drizzle, testes e outros tópicos.
4. Abra `.github/instructions/unit-tests.instructions.md` e observe `applyTo`. Esse campo define um padrão glob relativo à raiz do repositório; neste caso, abrange arquivos de teste TypeScript como `**/*.test.ts`.
5. Examine as instruções específicas para criar testes unitários.
6. Abra `.github/instructions/drizzle.instructions.md` e examine os links para instruções e arquivos do projeto. Esses links permitem dividir orientações em arquivos reutilizáveis e apontar exemplos; os caminhos são relativos ao arquivo de instruções.

> [!NOTE]
> **Code formatting requirements** em `copilot-instructions.md` documenta os padrões do projeto, mas ainda não exige documentação no código. Você adicionará comentários TSDoc e cabeçalhos de arquivo.

## Criar uma branch para as alterações

Crie uma branch para manter as alterações do exercício separadas.

1. Volte ao codespace.
2. Abra um terminal com <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie e selecione a branch:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar o Copilot Chat antes de atualizar as instruções

Envie um prompt com as instruções atuais e, depois de atualizar o arquivo, repita-o para comparar o resultado.

> [!TIP]
> **Abrir o Copilot Chat**
>
> No codespace, abra o painel Copilot Chat e selecione **New Chat** para começar uma conversa limpa. O modo e o modelo variam por exercício; cada etapa informa o que selecionar.

1. Feche as guias do editor abertas em exercícios anteriores para limitar o contexto.
2. Abra `src/lib/publishers.ts` para indicar onde o auxiliar deve ficar.
3. Na lista de agentes da visualização Chat, selecione **Agent** para permitir alterações nos arquivos.

   ![Seletor de agente na visualização Chat](../../../_images/shared-chat-mode-selector.png)

4. Envie este prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. O Copilot explorará o projeto e proporá alterações, possivelmente no auxiliar e nos testes.
6. Observe que a função proposta deve ser tipada, receber um cliente `db` como primeiro argumento e retornar uma matriz tipada de distribuidoras. Isso segue as convenções de `.github/instructions/drizzle.instructions.md`, aplicáveis a `src/lib/*.ts`.
7. Observe se o código proposto não inclui comentários TSDoc nem cabeçalho de arquivo.

> [!CAUTION]
> Como o Copilot é probabilístico, talvez ele inclua comentários mesmo sem essa orientação. Isso não é um problema; o objetivo é observar a consistência após atualizar as instruções.

## Adicionar um padrão para o repositório

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize **Code formatting requirements**, que deve estar perto da linha 27, e observe que ainda não há uma regra de documentação no código.
3. Logo abaixo dos padrões existentes, adicione:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Instruções no nível do repositório são adequadas para padrões gerais da equipe; arquivos `*.instructions.md` atendem a linguagens, tipos de arquivo e tarefas específicas.

## Repetir o prompt e observar a diferença

1. No Copilot Chat, selecione **New Chat**.
2. Volte para `src/lib/publishers.ts`.
3. Envie exatamente o mesmo prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observe se o arquivo proposto agora começa com um comentário semelhante a:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observe se a função inclui um comentário TSDoc semelhante a:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Você orientou o Copilot a seguir um novo padrão no código que servirá de base para o próximo exercício.

## Fazer commit das instruções e enviar a branch

1. Abra um terminal com <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Confirme que as instruções e o auxiliar foram alterados:

   ```bash
   git status
   ```

3. Prepare e faça commit das instruções e dos arquivos alterados pelo Copilot para o auxiliar:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Se git status mostrar outras alterações relacionadas (por exemplo, testes), adicione esses arquivos também.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envie a branch:

   ```bash
   git push -u origin custom-instructions
   ```

## Criar e fazer merge de um pull request

Com a branch publicada, crie um pull request e associe-o à issue sobre o padrão de documentação. As ferramentas do GitHub conectadas ao projeto permitem pedir ao Copilot que encontre a issue, crie o PR e o integre.

> [!NOTE]
> Este é o primeiro exercício em que o Copilot executa ações no GitHub. O modelo do projeto já configura as ferramentas do GitHub. Na primeira utilização, o VS Code talvez peça que você entre na sua conta e autorize o acesso. Você conhecerá a conexão (Model Context Protocol) em um exercício posterior.

1. Abra o Copilot Chat no codespace.
2. Pressione <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir a visualização Copilot Chat e confirme que **Agent** está selecionado.
3. Peça ao Copilot que encontre a issue e crie um pull request (PR) a partir da branch atual, descrevendo as instruções e o auxiliar de distribuidoras:

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Aguarde enquanto o Copilot localiza a issue e cria o PR. Quando solicitado, revise a ação do GitHub e selecione **Allow** se apropriado.
5. Quando o PR estiver criado, peça ao Copilot que faça merge e retorne a branch local para `main`:

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. Revise as solicitações de autorização para ações do GitHub e comandos do shell antes de selecionar **Allow**.

Você criou e integrou um pull request com a ajuda do GitHub Copilot.

## Resumo e próximos passos

Você explorou como o Copilot obtém contexto dos arquivos de instruções e usou o Copilot Chat no VS Code para comparar a geração de código antes e depois de adicionar um padrão ao repositório. Você também fez commit das instruções e do auxiliar para que o próximo exercício possa usá-los.

Em seguida, você usará as instruções no [modo de agente][next-lesson] para adicionar um recurso em várias partes do código.

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções melhores para o Copilot][copilot-instructions-five-tips]
- [Instruções pessoais para o GitHub Copilot][personal-instructions]
- [Awesome Copilot — coleção de arquivos de instruções][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
