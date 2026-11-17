---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos últimos exercícios, você atribuiu ao agente de nuvem do Copilot três tarefas para melhorar a experiência e adicionar funcionalidades. Embora o agente trabalhe de forma assíncrona e autônoma, é importante acompanhar o trabalho.

A [página de agentes][agents-page] do GitHub.com permite ver tarefas com pull requests abertos, explorar as operações realizadas e orientar uma sessão em andamento.

Neste exercício, você vai:

- explorar a página de agentes para acompanhar as tarefas do agente de nuvem.
- orientar uma sessão em andamento para solicitar funcionalidade adicional.

## Cenário

Depois de atribuir a implementação do modo de alto contraste, a equipe decidiu adicionar também um modo claro. Como o agente já está atualizando os estilos e criando controles de alternância, peça que inclua o modo claro no trabalho.

## Revisar as tarefas do agente de nuvem

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Examine a lista de tarefas nos painéis principal e esquerdo. Você deverá encontrar as tarefas que atribuiu ao Copilot, incluindo:
   - atualizar a documentação da base de código.
   - gerar APIs para modificar jogos.
   - adicionar um modo de alto contraste ao site.
3. Selecione uma tarefa em andamento e examine as operações realizadas pelo Copilot. Elas podem incluir:
   - obter o código do repositório.
   - criar o ambiente de trabalho do Copilot.
   - configurar servidores MCP.
   - executar várias etapas para concluir a tarefa.

> [!NOTE]
> As etapas exibidas variam de acordo com o estado da tarefa e a abordagem do Copilot.

4. Observe também o painel do pull request (PR) à direita. Ele permite examinar o PR e os arquivos alterados para acompanhar o trabalho.

## Orientar o agente de nuvem

Peça ao Copilot que adicione um controle para o modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão relacionada à implementação do modo de alto contraste. O título exato varia conforme o nome escolhido pelo Copilot e o estado do trabalho.

   ![Sessão de acessibilidade no painel de agentes](../../../_images/ex6-accessibility-session.png)

2. Acompanhe a sessão por alguns minutos, até que a configuração seja concluída e o agente comece a trabalhar. Você verá mensagens semelhantes às exibidas no exemplo do workshop.
3. Na caixa de diálogo **Steer active session while Copilot is working**, insira:

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Tarefa do agente de nuvem com a caixa de diálogo de orientação da sessão ativa](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe que o Copilot reconhece o prompt e o inclui no fluxo de trabalho.

## Deixar o Copilot trabalhar

Como antes, o Copilot continuará o trabalho na tarefa atualizada. Ele incorporará o novo pedido depois de concluir a etapa que estava executando quando você enviou a mensagem. Isso pode levar alguns minutos; aproveite o tempo para refletir sobre o que aprendeu.

## Resumo e próximos passos

Você explorou a página de agentes do Copilot, o painel central para acompanhar e orientar tarefas do agente de nuvem. Você acompanhou as tarefas e redirecionou uma sessão em andamento para solicitar outra funcionalidade.

Enquanto o Copilot conclui os recursos de acessibilidade, você [iterará sobre os pull requests que ele criou][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre agentes personalizados][custom-agents]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
