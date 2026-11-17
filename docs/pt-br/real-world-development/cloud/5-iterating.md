---
title: "Exercício 5 - Iterar sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revisar o trabalho

Durante este laboratório, você pediu ao GitHub Copilot para documentar o código, implementar jogos relacionados e criar recursos de acessibilidade, incluindo modos de alto contraste e claro. Revise as alterações e, se necessário, dê feedback ao Copilot.

### Cenário

Os fundamentos do projeto de software e do DevOps não mudam com a IA generativa. Revise o código gerado e siga o processo de DevOps habitual. Examine as sugestões do Copilot antes de habilitar a revisão para o restante da equipe.

## Segurança e agente de nuvem do GitHub Copilot

Como o agente de nuvem trabalha de forma assíncrona e sem supervisão direta, há restrições de segurança:

- O Copilot tem acesso de leitura ao repositório e acesso de gravação **somente** à branch que usará para o código.
- O agente trabalha em um ambiente separado e temporário no GitHub Actions.
- Workflows do GitHub Actions exigem aprovação humana antes de serem executados.
- O acesso a recursos externos, inclusive a servidores do Model Context Protocol (MCP), é [limitado por padrão][agent-firewall].

## Revisar a documentação gerada

Comece pelo primeiro pull request (PR) criado pelo agente de nuvem, que adiciona documentação ao código.

> [!NOTE]
> O PR pode mostrar um aviso de que o Copilot foi bloqueado por um firewall. Isso é esperado: por padrão, o acesso do Copilot a recursos externos, inclusive servidores MCP, é limitado. Você pode [personalizar ou desabilitar o firewall do agente de nuvem][agent-firewall].

1. Volte ao repositório no github.com.
2. Selecione **Pull Requests** para abrir a lista de pull requests.
3. Abra o pull request com título semelhante a **Add missing documentation** ou outro título mais específico.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o pull request terá a marca **[WIP]**. Aguarde a conclusão, o que pode levar alguns minutos. Enquanto isso, faça uma pausa ou reflita sobre o que aprendeu até aqui.

4. Quando o pull request estiver pronto, selecione **Files changed** e revise as alterações.

   ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Examine o código atualizado, incluindo os comentários TSDoc e a documentação adicionada. As alterações exatas variam. Procure comentários TSDoc, convenções de TypeScript e cabeçalhos de arquivo definidos nos arquivos de instruções personalizados examinados no início do Exercício 2.
6. Depois da revisão, volte à guia **Conversation** e role a página para baixo.
7. Verifique se há um indicador de workflows aguardando aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

9. Confirme que os workflows foram adicionados à seção de verificações do pull request e que as verificações do aplicativo Astro foram aprovadas. Isso pode levar alguns minutos.

## Solicitar alterações ao GitHub Copilot

Trabalhar com o Copilot em um pull request não é uma via de mão única. Você pode mencioná-lo em comentários do PR ou em comentários em linha no código. O Copilot verá esses comentários e iniciará outra sessão para atender ao pedido. Como os resultados são não determinísticos, não há um texto único a recomendar. Você pode pedir ao Copilot que:

- adicione no início de cada arquivo um cabeçalho de comentário com uma breve descrição da finalidade.
- adicione comentários TSDoc a arquivos TypeScript e Astro.
- crie um README que descreva a estrutura do aplicativo Astro.

1. Na conversa do PR, publique um comentário solicitando uma alteração e mencione **@copilot**.
2. Selecione **View Session** para acompanhar a nova sessão.
3. Para voltar ao PR, selecione **Back to pull request**.

   ![Voltar ao pull request](../../../_images/ex7-back-to-pr.png)

4. Quando o Copilot concluir as alterações, confirme que um novo commit foi adicionado ao pull request.
5. Selecione **Files changed** e revise as alterações.
6. Continue iterando até ficar satisfeito. Depois, marque o PR de rascunho como pronto para revisão e faça o merge na branch principal.

   ![Marcar o PR como pronto para revisão](../../../_images/ex7-ready-for-review.png)

## Revisar o recurso de jogos relacionados

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Abra o PR com título semelhante a **Show related games on the game details page** ou outro título mais específico.
4. Selecione **Files changed** para revisar o código gerado.
5. Depois da revisão, volte à guia **Conversation** e role a página para baixo.
6. Verifique se há um indicador de workflows aguardando aprovação.
7. Se houver, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

8. Confirme que os workflows foram adicionados à seção de verificações do pull request e que as verificações do aplicativo Astro foram aprovadas. Isso pode levar alguns minutos.
9. **Opcional:** mude para essa branch no Codespace para testar manualmente o recurso de jogos relacionados. Abra o terminal e substitua `<branch-name>` pelo nome da branch criada pelo Copilot, por exemplo, `copilot/fix-8`:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

O Copilot criou o recurso de jogos relacionados. Você pode continuar iterando e pedir, por exemplo, que ajuste a quantidade de jogos exibidos ou adicione cabeçalhos de comentário e comentários TSDoc. Lembre-se de que essa issue foi atribuída antes de você atualizar as instruções personalizadas. Para solicitar alterações, adicione outro comentário na guia **Conversation**; o Copilot o verá e iniciará uma nova sessão.

## Revisar os recursos de acessibilidade

O PR do agente personalizado deve conter o modo de alto contraste atribuído no Exercício 3 e o modo claro solicitado no Exercício 4.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Abra o PR com título semelhante a **Add high contrast mode to website** ou outro título mais específico.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o pull request terá a marca **[WIP]**. Aguarde a conclusão; isso pode levar alguns minutos.

4. Selecione **Files changed** para revisar o código gerado.
5. Revise a implementação, prestando atenção especial a:
   - controles da interface para alternar entre os modos.
   - uso do armazenamento local para persistir as preferências.
   - alterações de CSS e estilos para os modos de alto contraste e claro.
   - atributos de acessibilidade, como rótulos ARIA e navegação por teclado.
   - código JavaScript/TypeScript que controla a alternância dos modos.

6. Depois da revisão, volte à guia **Conversation** e role a página para baixo.
7. Verifique se há um indicador de workflows aguardando aprovação.
8. Se houver, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

9. Confirme que os workflows foram adicionados à seção de verificações do pull request e que as verificações do aplicativo Astro foram aprovadas. Isso pode levar alguns minutos.
10. **Opcional:** mude para essa branch no Codespace para testar manualmente os recursos de acessibilidade. Substitua `<branch-name>` pelo nome da branch criada pelo Copilot:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   Inicie o aplicativo e teste os controles dos modos de alto contraste e claro no navegador. Confirme que as preferências persistem após recarregar a página.

O agente personalizado de acessibilidade orientou a implementação com práticas recomendadas. Se encontrar problemas ou melhorias, mencione **@copilot** em um comentário para solicitar alterações.

## Exercício opcional: continue delegando

O agente de nuvem é útil para tarefas reais do backlog. Crie mais issues no repositório e atribua-as ao Copilot. Por exemplo:

- Criar um formulário de interesse de apoiadores na página de detalhes do jogo.
- Implementar paginação na lista de jogos.
- Adicionar validação de entrada e tratamento de erros aos auxiliares de acesso a dados.

## Resumo

Você concluiu o percurso do agente de nuvem. Ao longo das lições, você:

- examinou os arquivos de instruções incluídos no repositório para observar mais tarde seu efeito na saída do agente de nuvem.
- atribuiu issues ao agente de nuvem do Copilot e acompanhou a configuração do ambiente, o planejamento e a execução assíncrona.
- criou e usou um agente personalizado para implementar controles de alto contraste e modo claro.
- usou a página de agentes do Copilot como painel central para acompanhar e orientar a sessão de acessibilidade em andamento.
- revisou e refinou os pull requests do agente de nuvem, mencionando `@copilot` para solicitar alterações e aprovando workflows.

## Revisão e próximos passos

Para continuar explorando, os outros percursos complementam o que você praticou:

- [Percurso do VS Code](../../vscode/): explore o modo de agente do Copilot Chat e a integração com MCP no IDE.
- [Percurso do CLI](../../cli/): entregue alterações revisadas usando os modos Plan e Autopilot, skills, um agente de QA, MCP do Playwright, Agent Merge e a delegação opcional com `/delegate`.

No próprio repositório, atribua uma issue de refatoração ou cobertura de testes ao agente de nuvem, crie um agente personalizado para outro domínio ou configure `copilot-setup-steps.yml` para outra pilha tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre agentes do Copilot][copilot-agents]
- [Visão geral do agente de nuvem][cloud-agent-overview]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas do workflow de configuração do agente de nuvem][cloud-agent-best-practices]
- [Configurar o firewall do agente de nuvem][agent-firewall]

---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
