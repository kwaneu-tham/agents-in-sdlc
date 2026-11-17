---
title: "Exercício 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents] do GitHub Copilot são assistentes de IA especializados em tarefas ou domínios do fluxo de desenvolvimento. Arquivos Markdown na pasta `.github/agents` do repositório fornecem instruções, práticas recomendadas, padrões de código e conhecimento de domínio. As equipes podem codificar sua experiência em agentes reutilizáveis, como agentes de acessibilidade que seguem as [Diretrizes de Acessibilidade para Conteúdo Web (WCAG)][wcag], agentes de segurança ou agentes de testes.

Os agentes personalizados são definidos em arquivos Markdown na pasta `.github/agents` do projeto ou globalmente em `~/.copilot/agents`. Cada arquivo tem frontmatter YAML com pelo menos `name` e `description`, seguido de um prompt Markdown que define o comportamento, a especialidade e as instruções do agente.

### Agentes personalizados e skills de agente

Agentes personalizados e [skills de agente][agent-skills] têm semelhanças: ambos são definidos principalmente em arquivos Markdown e orientam uma IA sobre como executar tarefas. Uma distinção útil é que o **agente personalizado** é quem executa o trabalho, enquanto as **skills** são ferramentas.

Agentes personalizados têm sua própria janela de contexto e podem coordenar skills e outros agentes. Neste laboratório, o agente de acessibilidade revisará e atualizará o site conforme as diretrizes de acessibilidade e poderá usar skills de fluxo de pull request ou de testes.

> [!NOTE]
> Não há uma única maneira correta de criar um agente personalizado. Teste e refine a configuração para os ambientes e cenários em que será usada.

[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

Com agentes personalizados, você explorará como:

- definir agentes personalizados.
- atribuir uma tarefa a um agente personalizado.

## Cenário

A Tailspin Toys quer que sua plataforma de crowdfunding seja acessível a todas as pessoas, independentemente de suas habilidades ou preferências visuais. Pessoas usuárias relataram dificuldade para ler o tema escuro atual por causa do contraste insuficiente entre texto e fundo. A equipe de design pediu um modo de alto contraste que possa ser ativado e desativado. Como acessibilidade é importante, você usará um agente personalizado para implementar esse recurso.

## Revisar o agente personalizado de acessibilidade

Um agente personalizado de acessibilidade já foi criado no projeto. Revise-o para entender como orientará o Copilot.

1. Volte ao codespace e abra `.github/agents/accessibility.md`.
2. Observe os campos `name` e `description` no frontmatter YAML.

> [!CAUTION]
> Os campos `name` e `description` no frontmatter são obrigatórios para agentes personalizados.

3. Examine as seções seguintes: responsabilidades principais, práticas recomendadas de acessibilidade, exemplos de HTML, CSS e JavaScript e erros comuns.

## Criar e atribuir uma issue

O painel de agentes é o local central para atribuir tarefas ao agente de nuvem, acompanhar o trabalho e fornecer orientações adicionais. Crie uma issue para o modo de alto contraste e atribua-a ao agente personalizado.

1. Acesse o repositório e selecione a guia **Issues**.
2. Selecione **New issue** e depois **Blank issue**.
3. No campo **Title**, digite `Add high contrast mode to website`.
4. No campo **Description**, insira:

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

5. Selecione **Create** e, à direita, **Assign to Copilot**.
6. Na lista de agentes personalizados, selecione **Accessibility agent** e depois **Assign**.

   ![Caixa de atribuição do agente de nuvem com o agente personalizado Accessibility selecionado](../../../_images/ex5-select-custom-agent.png)

O Copilot começará a trabalhar na tarefa em segundo plano.

## Resumo e próximos passos

Você explorou [agentes personalizados][custom-agents], assistentes especializados que permitem reutilizar a experiência e os padrões da equipe. Nesta lição, você examinou como agentes personalizados são definidos e atribuiu uma tarefa ao agente de acessibilidade.

Enquanto o Copilot implementa o modo de alto contraste, você [acompanhará e orientará a sessão do agente][next-lesson] no painel de agentes.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Preparar o uso de agentes personalizados na organização][org-custom-agents]
- [Preparar o uso de agentes personalizados na empresa][enterprise-custom-agents]

---

| [← Lição anterior: Agente de nuvem do GitHub Copilot][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
