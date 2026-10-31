---
title: "Exercício 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

É provável que quase todas as organizações tenham dificuldades com dívida técnica. Ela pode incluir problemas de segurança não resolvidos, código legado que precisa de atualizações ou solicitações de recursos que ficaram paradas no backlog por falta de tempo para implementá-las. O cloud agent do GitHub Copilot foi criado para realizar tarefas como atualizar código e adicionar funcionalidades de forma autônoma. Quando termina o trabalho, ele gera um rascunho de pull request (PR) pronto para a revisão de um desenvolvedor. Isso permite delegar tarefas tediosas, acelerar o desenvolvimento e liberar os desenvolvedores para se concentrarem em questões mais amplas.

Neste exercício, você explorará como usar o Copilot cloud agent para:

- personalizar o ambiente de geração de código;
- garantir que as operações sejam realizadas com segurança;
- entender a importância de definir issues com escopo claro;
- atribuir issues ao Copilot.

## Cenários

A Tailspin Toys tem algumas dívidas técnicas que gostaria de resolver. Os prestadores de serviço contratados inicialmente para criar a primeira versão do site deixaram a documentação em um estado insatisfatório: você perceberá que ela está completamente ausente. Como primeira etapa, a equipe quer adicionar comentários de documentação do TypeScript (TSDoc) a todas as funções exportadas do aplicativo.

Além disso, a equipe de design está pronta para aprimorar a descoberta de jogos. Ela quer que a página de detalhes de cada jogo mostre jogos relacionados — outros títulos da mesma categoria — para que as pessoas possam continuar explorando. Por enquanto, a equipe não precisa de uma implementação refinada; quer algo que possa usar para testar a experiência de usuário (UX) com critérios de aceitação. Isso é um bloqueio no momento, mas há outras issues com prioridade maior.

Esses são dois exemplos de tarefas que podem ser rebaixadas na prioridade com facilidade e que são ótimas candidatas para o Copilot cloud agent. O agente pode trabalhar nelas de forma assíncrona enquanto você se concentra em outras tarefas. Depois, você poderá voltar para revisar o trabalho e confirmar que está tudo como esperado.

## Apresentação do GitHub Copilot cloud agent

O [GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) pode executar tarefas em segundo plano, de maneira semelhante a um desenvolvedor. Assim como no trabalho com um desenvolvedor, há várias maneiras de atribuir tarefas, inclusive [atribuindo uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois da atribuição, o Copilot cria um pull request de rascunho para acompanhar o andamento, configura um ambiente e começa a trabalhar na tarefa. Você pode examinar a sessão do Copilot enquanto o trabalho está em andamento ou depois que ele termina. Quando a solução proposta estiver pronta para revisão, o Copilot marcará você no pull request.

## A importância de instruções com escopo bem definido

Não há mágica no GitHub Copilot: não é possível deixar de pensar. Até operações aparentemente simples se tornam complexas quando examinadas em detalhes. Por isso, [defina com cuidado o escopo das tarefas do Copilot cloud agent][cloud-agent-best-practices]. Trate-o como um parceiro de programação com IA: trabalhe em etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software não mudam com a chegada da IA generativa.

## Instruções personalizadas neste repositório

Nos exercícios anteriores, você conheceu as instruções personalizadas e como elas orientam o Copilot. Se já concluiu o exercício prático sobre instruções personalizadas, viu o arquivo `.github/copilot-instructions.md` em ação. Caso contrário, este é um bom momento para examiná-lo rapidamente. A leitura dos arquivos de instruções já incluídos neste repositório ajudará você a observar seus efeitos quando atribuir trabalho ao Copilot cloud agent.

Os arquivos a seguir mostram as instruções que você consultará na interface Web do GitHub ou em um codespace, se já tiver iniciado um:

1. Abra `.github/copilot-instructions.md` e examine a seção **Code standards**, principalmente as expectativas para convenções de TypeScript e comentários TSDoc.
2. Abra `.github/instructions/unit-tests.instructions.md` e observe o frontmatter `applyTo`, que aplica as instruções a arquivos `**/*.test.ts`.

Quando você atribuir a issue `Code lacks documentation` ao cloud agent, na próxima seção, observe se o pull request resultante contém comentários TSDoc, segue as convenções de TypeScript e inclui cabeçalhos de comentário. Essas orientações vêm dos arquivos de instruções que você está examinando. Voltaremos a esse assunto ao revisar os PRs em um exercício posterior.

## Configurar o ambiente de desenvolvimento do Copilot cloud agent

Todo trabalho de programação, independentemente de quem o realiza, normalmente exige um ambiente específico e a execução de scripts de configuração para garantir que tudo esteja pronto. O mesmo vale ao atribuir tarefas ao Copilot, que trabalha de maneira semelhante a um engenheiro de software.

O cloud agent usa o [GitHub Actions][github-actions] como ambiente de trabalho. Você pode personalizar esse ambiente criando um [workflow de configuração especial][setup-workflow] no arquivo `.github/workflows/copilot-setup-steps.yml`, para ser executado antes de o agente começar a trabalhar. Assim, ele terá acesso às ferramentas e dependências de desenvolvimento necessárias. Esse workflow já foi configurado para facilitar o fluxo do laboratório e permitir que você explore o recurso. Ele garante que o Copilot tenha acesso ao Node.js, às dependências do projeto, aos binários de navegador para testes de ponta a ponta e ao banco de dados SQLite migrado e preenchido com dados para o aplicativo Astro único:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões mínimas necessárias para *suas etapas*. O Copilot receberá um token próprio para as operações dele.
    permissions:
      # Para clonar o repositório durante as etapas de configuração, por exemplo, para instalar dependências, você precisará da permissão `contents: read`. Se não clonar o repositório nessas etapas, o Copilot fará isso automaticamente quando elas forem concluídas.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do front-end/aplicativo - Node.js (todo o aplicativo agora usa Astro + Drizzle/libSQL)
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

      # Migra e preenche o banco de dados SQLite local para que as compilações e os testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo parece qualquer outro workflow do GitHub, mas tem alguns pontos importantes:

- Ele contém um único trabalho chamado `copilot-setup-steps`. Esse trabalho é executado no GitHub Actions antes de o Copilot começar a trabalhar no pull request.
- Observe o gatilho `workflow_dispatch`, que permite executar o workflow manualmente na guia **Actions** do repositório. Isso é útil para testar se o workflow funciona corretamente, sem precisar esperar que o Copilot o execute.

## Adicionar documentação

Todo mundo entende a importância da documentação, mas muitos projetos têm informações desatualizadas ou não têm documentação. Esse é o tipo de dívida técnica que costuma ficar sem solução, reduzir a produtividade e dificultar a manutenção da base de código ou a integração de novos desenvolvedores à equipe. Felizmente, o Copilot é muito bom em criar documentação, e esta é uma issue perfeita para atribuir ao Copilot cloud agent. Ele trabalhará em segundo plano para gerar a documentação necessária. Em um exercício futuro, você voltará para revisar o trabalho.

1. Em uma nova guia do navegador, acesse seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para criar uma issue em branco.
5. Defina **Title** como `Code lacks documentation`.
6. Defina **Description** como:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecione **Create** para criar a issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

  ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

10. Selecione a guia **Pull Requests**.
11. Abra o pull request recém-gerado, que terá um título semelhante a `[WIP]: Code lacks documentation`. Se nenhum PR novo aparecer na lista, aguarde um ou dois minutos e atualize a janela do navegador.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas do Copilot pode levar vários minutos para aparecer no PR. O Copilot está preparando o ambiente, executando o workflow descrito anteriormente, analisando o projeto e determinando a melhor abordagem para resolver o problema.

13. Revise a lista e as tarefas que o Copilot planeja concluir.
14. Role a linha do tempo do pull request para baixo. Você deverá ver uma atualização informando que o Copilot começou a trabalhar na issue.
15. Selecione o botão **View session**.

  ![Exibição da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Observe que você pode percorrer a sessão em tempo real e acompanhar como o Copilot resolve o problema. Ele explora o código e entende o estado atual, faz pausas para analisar e definir um plano e cria código.

Esse processo provavelmente levará alguns minutos. Um dos principais objetivos do Copilot cloud agent é permitir que ele execute tarefas de forma assíncrona e liberar você para se concentrar em outras atividades. Vamos aproveitar esse recurso atribuindo outra tarefa ao cloud agent e, em seguida, voltando nossa atenção à criação de funcionalidades para o aplicativo.

## Adicionar uma seção de jogos relacionados à página de detalhes do jogo

Como vimos, uma das grandes vantagens do GitHub Copilot cloud agent é a possibilidade de dividir o trabalho: você pode se concentrar em um conjunto de tarefas enquanto ele trabalha em outro. Talvez a criação de uma seção de jogos relacionados para a equipe de design não demore muito, mas esse tempo ainda poderia ser usado em outras tarefas. Vamos atribuí-la ao Copilot cloud agent.

1. Volte ao seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para usar o template em branco.
5. Defina **Title** como `Show related games on the game details page`.
6. Defina **Description** como:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. Selecione **Create** para criar a issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

  ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Logo depois, você deverá ver um emoji 👀 no primeiro comentário da issue, indicando que o Copilot está trabalhando nela.

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

Agora o Copilot está trabalhando na segunda solicitação. O Copilot cloud agent funciona de maneira semelhante a um engenheiro de software, então não é necessário monitorá-lo continuamente. Você poderá revisar o trabalho quando ele terminar. Vamos voltar nossa atenção à criação e ao uso de agentes personalizados.

## Resumo e próximas etapas

Este exercício explorou o [GitHub Copilot cloud agent][copilot-agents]. Com o cloud agent, você pode atribuir issues ao Copilot para que ele trabalhe nelas de forma assíncrona. Você pode usá-lo para resolver dívidas técnicas, criar funcionalidades ou ajudar a migrar código de uma estrutura para outra.

Você explorou estes conceitos:

- personalizar o ambiente de geração de código;
- garantir que as operações sejam realizadas com segurança;
- entender a importância de definir issues com escopo claro;
- atribuir issues ao Copilot.

Enquanto o cloud agent trabalha em segundo plano, vamos nos concentrar em criar e usar agentes personalizados. O [Copilot cloud agent também pode usar servidores MCP][cloud-agent-mcp] e pode acessar instruções personalizadas, que já exploramos em módulos anteriores.

## Recursos

- [Sobre o Copilot cloud agent][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do Copilot cloud agent][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
