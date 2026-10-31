---
title: "Exercício 5: Iterar no trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Monitorar e gerenciar agentes][previous-lesson] |
|:--|

## Revisar o trabalho

Durante este laboratório, você trabalhou com o GitHub Copilot em várias tarefas para aprimorar a experiência de usuário e adicionar funcionalidades. Você pediu ao Copilot para adicionar documentação ao código, criar um recurso de jogos relacionados para a equipe de design explorar e implementar recursos de acessibilidade, incluindo controles para os modos de alto contraste e claro. Vamos examinar as alterações e, se necessário, enviar comentários ao Copilot para aprimorar o trabalho.

### Cenário

Como já destacamos várias vezes, os fundamentos do design de software e do desenvolvimento e operações (DevOps) não mudam com a chegada da inteligência artificial (IA) generativa. Sempre queremos revisar o código gerado e seguir nosso processo normal de DevOps. Com isso em mente, vamos revisar as sugestões do GitHub Copilot para criar a documentação, o recurso de jogos relacionados e os recursos de acessibilidade antes de habilitar a revisão pelo restante da equipe.

## Segurança e GitHub Copilot cloud agent

Como o Copilot cloud agent executa tarefas de forma assíncrona e sem supervisão, há algumas restrições de segurança para garantir que tudo continue seguro. Entre elas:

- O Copilot tem acesso de leitura ao repositório e acesso de gravação **somente** à ramificação usada para o código.
- O cloud agent é executado no GitHub Actions, que cria um ambiente separado e temporário para o trabalho.
- Um humano precisa aprovar todos os workflows do GitHub Actions antes da execução.
- [O acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluindo o acesso a servidores MCP.

## Revisar a documentação gerada

Vamos começar examinando o primeiro pull request (PR) gerado pelo GitHub Copilot cloud agent: o PR que adiciona documentação ao código. Você fará isso na interface padrão de PRs do GitHub.com.

> [!NOTE]
> Ao abrir o PR, talvez você veja um aviso informando que o firewall bloqueou o GitHub Copilot. Isso é **esperado**, pois o Copilot tem acesso limitado a recursos externos por padrão, incluindo servidores MCP externos. Se quiser, você pode [personalizar ou desativar o firewall do Copilot cloud agent][agent-firewall].

1. Volte ao seu repositório no github.com.
2. Selecione **Pull Requests** para abrir a lista de pull requests.
3. Abra o pull request com um título semelhante a **Add missing documentation** ou outro título mais específico.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, o pull request terá o sinalizador **[WIP]**. Nesse caso, aguarde a conclusão do trabalho. Isso pode levar alguns minutos. Você pode fazer uma pausa ou refletir sobre o que aprendeu até agora.

4. Quando o pull request estiver pronto, selecione a guia **Files changed** e revise as alterações.

    ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Explore o código atualizado, que inclui os novos comentários TSDoc e outros elementos de documentação. As alterações exatas variam.

    Ao examinar as alterações, procure comentários TSDoc, convenções de TypeScript e cabeçalhos de comentário. Eles vêm dos arquivos de instruções personalizadas que você examinou no início do Exercício 2.

6. Depois de revisar as atualizações e confirmar que está tudo certo, volte à guia **Conversation** e role a página para baixo.
7. Você deverá ver um indicador de que alguns workflows estão aguardando aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

9. Os workflows deverão ser adicionados à fila na seção de verificações do pull request. Se tudo estiver correto, as verificações do projeto para o aplicativo Astro único serão aprovadas. Isso pode levar alguns minutos.

## Solicitar alterações ao GitHub Copilot

Trabalhar com o Copilot em um pull request não é uma via de mão única. Você também pode mencioná-lo em comentários, como faria com qualquer pessoa da equipe, no pull request ou em comentários em linha no código. O Copilot verá esses comentários e iniciará outra sessão para resolvê-los. Como os resultados não são determinísticos, não podemos recomendar um texto específico. Algumas sugestões do que pedir ao Copilot:

- adicionar cabeçalhos de comentário no início de cada arquivo de código, com uma breve descrição do conteúdo;
- adicionar comentários TSDoc a arquivos TypeScript e Astro;
- criar um arquivo README com uma descrição da estrutura do aplicativo Astro.

1. Adicione um comentário solicitando uma alteração na documentação gerada e mencione **@copilot**, como faria com qualquer usuário. Use uma das sugestões acima ou peça outra melhoria relacionada à documentação do código.
2. Selecione **View Session** para acompanhar o trabalho do Copilot. Observe como ele inicia uma nova sessão para fazer as atualizações.
3. Você pode selecionar **Back to pull request** para voltar ao pull request.

    ![Voltar ao pull request](../../../_images/ex7-back-to-pr.png)

4. Quando o Copilot concluir as alterações, deverá aparecer um novo commit no pull request.
5. Selecione a guia **Files changed** para revisar as alterações.

Continue fazendo iterações até ficar satisfeito. Depois, você poderá converter o PR de rascunho em pronto para revisão e mesclá-lo à ramificação principal.

![Converter o PR em pronto para revisão](../../../_images/ex7-ready-for-review.png)

## Revisar o recurso de jogos relacionados

Agora, vamos voltar ao PR criado pelo Copilot para resolver a issue sobre a exibição de jogos relacionados na página de detalhes de um jogo.

1. Volte ao seu repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título semelhante a **Show related games on the game details page** ou outro título mais específico.
4. Selecione a guia **Files changed** para revisar o código gerado.
5. Depois de revisar as atualizações e confirmar que está tudo certo, volte à guia **Conversation** e role a página para baixo.
6. Você deverá ver um indicador de que alguns workflows estão aguardando aprovação.
7. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

8. Os workflows deverão ser adicionados à fila na seção de verificações do pull request. Se tudo estiver correto, as verificações do projeto para o aplicativo Astro único serão aprovadas. Isso pode levar alguns minutos.
9. **Opcional:** você também pode mudar para essa ramificação no codespace para testar manualmente o recurso de jogos relacionados. Acesse o codespace, abra o terminal e execute os comandos a seguir (substitua `<branch-name>` pelo nome da ramificação criada pelo Copilot, por exemplo, **copilot/fix-8**):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

O Copilot criou o recurso de jogos relacionados. Como antes, você pode trabalhar iterativamente com o Copilot cloud agent para solicitar atualizações. Por exemplo, você pode pedir que o Copilot ajuste o número de jogos relacionados exibidos ou adicione cabeçalhos de comentário e comentários TSDoc. Lembre-se: essa tarefa foi atribuída **antes** da atualização das instruções personalizadas. Como nos casos anteriores, você pode fazer essas solicitações adicionando um comentário na guia **Conversation**. O Copilot verá o comentário e iniciará uma nova sessão.

## Revisar os recursos de acessibilidade

Por fim, vamos revisar os recursos de acessibilidade implementados com o agente personalizado de acessibilidade. Este PR deve incluir tanto o modo de alto contraste atribuído no Exercício 3 quanto o modo claro solicitado no painel de controle do Exercício 4.

1. Volte ao seu repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título semelhante a **Add high contrast mode to website** ou outro título mais específico.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, o pull request terá o sinalizador **[WIP]**. Nesse caso, aguarde a conclusão do trabalho, que pode levar alguns minutos.

4. Selecione a guia **Files changed** para revisar o código gerado.
5. Revise a implementação, prestando atenção especial a:
   - componentes da interface para alternar entre os modos;
   - uso do armazenamento local para manter as preferências do usuário;
   - alterações de folhas de estilo em cascata (CSS) ou de estilo dos modos de alto contraste e claro;
   - atributos de acessibilidade, como rótulos de Aplicações Ricas de Internet Acessíveis (ARIA) e navegação pelo teclado;
   - código JavaScript/TypeScript que gerencia a alternância entre os modos.

6. Depois de revisar as atualizações e confirmar que está tudo certo, volte à guia **Conversation** e role a página para baixo.
7. Você deverá ver um indicador de que alguns workflows estão aguardando aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

9. Os workflows deverão ser adicionados à fila na seção de verificações do pull request. Se tudo estiver correto, as verificações do projeto para o aplicativo Astro único serão aprovadas. Isso pode levar alguns minutos.
10. **Opcional:** você pode mudar para essa ramificação no codespace e testar manualmente os recursos de acessibilidade. Acesse o codespace, abra o terminal e execute os comandos a seguir (substitua `<branch-name>` pelo nome da ramificação criada pelo Copilot):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Em seguida, inicie o aplicativo e teste os controles dos modos de alto contraste e claro no navegador para confirmar se funcionam como esperado e se as preferências continuam salvas após recarregar as páginas.

Observe como o agente personalizado de acessibilidade ajudou a orientar o Copilot para implementar esses recursos seguindo práticas recomendadas de acessibilidade. Se você encontrar problemas ou oportunidades de melhoria, mencione **@copilot** em um comentário para solicitar atualizações, como fez com os outros PRs.

## Exercício opcional — continuar delegando

O cloud agent funciona melhor quando você pode delegar itens reais do backlog e se concentrar em outras tarefas. Para desenvolver esse hábito, crie mais algumas issues no seu repositório e atribua-as ao Copilot. Algumas ideias:

- criar um formulário de interesse para apoiadores na página de detalhes do jogo;
- implementar paginação na página de lista de jogos;
- adicionar validação de entrada e tratamento de erros aos auxiliares de acesso a dados.

## Resumo

Você concluiu o fluxo do cloud agent. Ao longo destas lições, você:

- **Examinou os arquivos de instruções personalizadas incluídos no repositório** para ver como eles influenciam os resultados do cloud agent.
- **Atribuiu issues ao Copilot cloud agent** e acompanhou a configuração do ambiente, o planejamento e a execução assíncrona.
- **Criou e usou um agente personalizado** para acessibilidade, adicionando controles para os modos de alto contraste e claro.
- **Usou a página de agentes do Copilot como painel de controle** para monitorar e orientar a sessão de acessibilidade em andamento.
- **Revisou e aprimorou os pull requests do cloud agent**, mencionando `@copilot` para solicitar alterações e aprovando workflows.

## Revisão e próximas etapas

Você concluiu o fluxo do cloud agent. Se quiser continuar explorando, os outros fluxos complementam o que você praticou aqui:

- 🖥️ **[Fluxo do VS Code](../../vscode/)** — explore o modo de agente do Copilot Chat e a integração com MCP diretamente no IDE.
- 💻 **[Fluxo do CLI](../../cli/)** — entregue duas alterações revisadas no terminal usando os modos Plan e Autopilot, habilidades de agente, um agente de QA personalizado, MCP do Playwright, Agent Merge e uma ponte `/delegate` opcional para o cloud agent.

No seu repositório, experimente estas ideias:

- atribuir ao cloud agent uma issue de refatoração ou cobertura de testes;
- criar um agente personalizado para outro domínio;
- configurar `copilot-setup-steps.yml` para outra pilha de tecnologias.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre os agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do Copilot cloud agent][cloud-agent-best-practices]
- [Configurar o firewall do Copilot cloud agent][agent-firewall]

---

| [← Lição anterior: Monitorar e gerenciar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
