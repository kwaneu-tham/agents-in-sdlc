---
title: "Exercício 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents] do GitHub Copilot são assistentes de IA especializados, adaptados a tarefas ou domínios específicos do fluxo de desenvolvimento. Ao defini-los em arquivos Markdown na pasta `.github/agents` do repositório, você fornece ao Copilot instruções específicas, práticas recomendadas, padrões de código e conhecimento de domínio que o ajudam a realizar determinados tipos de trabalho com mais eficácia. As equipes podem codificar sua experiência em agentes reutilizáveis: por exemplo, um agente de acessibilidade que aplica as diretrizes [WCAG][wcag], um agente de segurança que segue práticas de codificação segura ou um agente de testes que mantém padrões de teste consistentes.

Os agentes são definidos em arquivos Markdown na pasta `.github/agents` do projeto ou globalmente em `~/.copilot/agents`. Cada arquivo tem frontmatter YAML com pelo menos os campos `name` e `description`, seguido de um prompt Markdown que define o comportamento, a especialidade e as instruções do agente.

### Agentes personalizados e skills de agente

Agentes personalizados e [skills de agente][agent-skills] têm semelhanças: ambos são definidos principalmente em arquivos Markdown e orientam uma IA sobre como executar tarefas. Uma distinção útil é que o **agente personalizado** executa o trabalho, enquanto as **skills** são ferramentas.

Agentes personalizados têm sua própria janela de contexto e podem coordenar skills e outros agentes. Neste laboratório, o agente de acessibilidade revisará e atualizará o site conforme as diretrizes e poderá usar skills de fluxo de pull request ou de testes.

> [!NOTE]
> Não há uma única maneira correta de criar um agente. Teste e refine a configuração para os ambientes e cenários em que será usada.

[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Cenário

A Tailspin Toys quer que sua plataforma seja acessível a todas as pessoas, independentemente de suas habilidades ou preferências visuais. Pessoas usuárias relataram dificuldade para ler o tema escuro por causa do contraste insuficiente. A equipe de design pediu um modo de alto contraste que possa ser ativado e desativado. Você usará um agente personalizado para implementar esse recurso.

Neste exercício, você vai:

- revisar um agente personalizado de acessibilidade existente.
- usar o agente de acessibilidade no Copilot Chat para implementar o modo de alto contraste.

## Revisar o agente personalizado de acessibilidade

Volte ao codespace e crie uma branch a partir de `main` para manter o trabalho de acessibilidade separado do PR de filtragem.

1. Abra um terminal e execute:

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

2. Abra `.github/agents/accessibility.md` e observe os campos `name` e `description` no frontmatter YAML.

> [!CAUTION]
> Os campos `name` e `description` no frontmatter são obrigatórios para agentes personalizados.

3. Examine as responsabilidades, as práticas de acessibilidade, os exemplos de HTML, CSS e JavaScript e os erros comuns.

## Usar o agente personalizado no Copilot Chat

O VS Code mostra os agentes personalizados de `.github/agents` na lista de agentes na parte inferior do Copilot Chat. Selecione um agente para aplicar suas instruções e ferramentas à conversa.

> [!TIP]
> **Abrir o Copilot Chat**
>
> No codespace, abra o Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam por exercício.

1. Na lista de agentes da visualização Chat, selecione **Agent**, caso ainda não esteja selecionado.

   ![Seletor de agente na visualização Chat](../../../_images/shared-chat-mode-selector.png)

2. Abra a lista de agentes na parte inferior do chat, onde o agente ativo aparece como **default**.
3. Selecione **Accessibility agent**.
4. Envie este prompt:

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Acompanhe o Copilot enquanto ele explora o código e propõe alterações. Cada edição aparece no chat com o caminho do arquivo e um diff.
6. Observe o indicador **Files changed** para acompanhar os arquivos alterados.

> [!NOTE]
> O processo pode levar alguns minutos. O Copilot altera arquivos reais, como componentes Astro, CSS e testes relacionados. Você acompanhará e orientará o trabalho no próximo exercício.

## Resumo e próximos passos

Você revisou um agente personalizado e o usou no modo de agente do Copilot Chat. Em seguida, você [acompanhará e orientará o trabalho do agente][next-lesson], revisando as alterações e adicionando um controle de modo claro à mesma sessão.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Criar agentes personalizados no IDE][creating-custom-agents-ide]
- [Agentes personalizados no VS Code][custom-agents-vscode]
- [Configuração de agentes personalizados][custom-agents-config]
- [Agentes personalizados no awesome-copilot][awesome-copilot-agents]

---

| [← Lição anterior: Testar o recurso com o MCP do Playwright][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
