---
title: "Exercício 5 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Agentes personalizados][previous-lesson]

No modo de agente, o Copilot explora o código, propõe alterações, edita arquivos e executa comandos. No VS Code, você acompanha chamadas de ferramentas e edições em tempo real, revisa diffs no chat, aceita ou rejeita alterações e orienta a conversa sem sair do editor.

Neste exercício, você vai:

- acompanhar em tempo real o trabalho do agente de acessibilidade.
- revisar e aceitar as edições propostas.
- orientar o agente durante a sessão para adicionar um controle do modo claro junto ao modo de alto contraste.

## Cenário

O agente personalizado de acessibilidade do [Exercício 4][previous-lesson] está implementando o modo de alto contraste. Enquanto o trabalho está em andamento, a equipe de design pede também um controle de modo claro. Em vez de começar uma conversa nova, oriente a sessão existente para ampliar o trabalho.

## Acompanhar o progresso do Copilot

1. Volte à visualização Copilot Chat do Exercício 4.
2. Enquanto o agente trabalha, acompanhe as chamadas de ferramentas, leituras e edições de arquivos e comandos do terminal.
3. Quando uma edição aparecer com um diff no chat, selecione **Keep** para aceitá-la ou **Undo** para revertê-la.
4. Abra **Files changed** no topo do chat para revisar o conjunto completo de arquivos alterados.

> [!NOTE]
> Se uma chamada de ferramenta pedir autorização, por exemplo, para executar um comando do terminal, o agente aguardará sua resposta em **Allow**. Aprove apenas comandos que você considera seguros.

5. Aguarde até que o agente conclua o modo de alto contraste e apresente um resumo das alterações.

## Orientar o agente para adicionar o modo claro

Como você continuará na mesma conversa, o agente manterá o contexto dos componentes, dos estilos e do padrão de armazenamento local que já criou.

1. No campo de entrada do Copilot Chat, envie:

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. Acompanhe o agente enquanto ele examina as alterações existentes e propõe as adições para o modo claro.
3. Revise cada nova edição e selecione **Keep** ou **Undo** no diff.
4. Se o agente seguir uma direção indesejada, como refatorar os controles de modo de uma forma que você não quer, interrompa-o com **Stop** ao lado do campo de entrada e envie uma orientação mais clara.

> [!TIP]
> Orientar uma sessão em andamento é mais rápido do que recomeçar. Use prompts complementares para pedir ajustes — por exemplo, simplificar o CSS usando variáveis CSS em vez de duplicar seletores —, corrigir a abordagem — como integrar o modo claro ao painel de configurações existente, em vez de adicionar um terceiro controle — ou ampliar o escopo, por exemplo, adicionando atalhos de teclado para os dois controles.

## Revisar o conjunto de alterações

Antes do commit, revise o trabalho do agente.

1. Abra a visualização **Source Control** no VS Code.
2. Revise os arquivos alterados; devem incluir componentes Astro, estilos e testes relacionados.
3. Abra alguns arquivos e examine os diffs. Confirme os atributos Accessible Rich Internet Applications (ARIA), a navegação por teclado, o HTML semântico e a persistência no armazenamento local.
4. Quando estiver satisfeito, prepare e faça commit das alterações no painel Source Control. Você as publicará na próxima lição.

## Resumo e próximos passos

Você acompanhou e orientou uma sessão em andamento no modo de agente do Copilot Chat, revisando edições e enviando prompts complementares sem perder o contexto. Em seguida, você publicará o trabalho de acessibilidade em um pull request e revisará as alterações produzidas pelo agente personalizado local na próxima lição.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo de agente][agent-mode]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
