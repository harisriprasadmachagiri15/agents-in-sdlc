---
title: "Lição 3 - Modos de agente: Plan e Autopilot"
description: "Use Plan para definir uma abordagem, Autopilot para implementar filtros a partir de uma issue e Interactive para revisar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Você começou adicionando uma pequena funcionalidade ao projeto. Mudanças maiores exigem um processo mais robusto. Nesta lição, você vai partir de uma issue, definir o que precisa ser feito e implementar e validar a funcionalidade antes de mesclá-la.

> [!NOTE]
> Você continuará o fluxo de trabalho da funcionalidade na mesma conversa e branch. Em geral, você usaria branches ou pull requests diferentes para cada tipo de arquivo, mas este workshop reúne o trabalho para que você se concentre nos conceitos principais.

Nesta lição, você vai:

- iniciar uma conversa do Copilot a partir de uma issue do GitHub.
- definir os requisitos no modo Plan.
- implementar a funcionalidade no modo Autopilot.
- revisar o código.
- validar a funcionalidade manualmente em um navegador encaminhado.

Ao longo do trabalho, você também atualizará as instruções do repositório, personalizará a skill `quality-checks`, adicionará a validação MCP, criará um agente de QA e abrirá o pull request da funcionalidade.

## Cenário

O catálogo da Tailspin Toys está crescendo, e os visitantes precisam filtrar os jogos por categoria e editora. A issue do backlog descreve a funcionalidade, mas decisões como combinar categorias precisam ser acordadas antes de escrever o código. Use o modo Plan para definir essas decisões e, em seguida, autorize uma implementação delimitada com o modo Autopilot.

## Contexto

O uso de agentes de programação com IA não muda os fundamentos do desenvolvimento. Um fluxo comum é:

1. Começar com uma issue que descreve o trabalho.
2. Planejar o que precisa ser desenvolvido.
3. Implementar e revisar o código.
4. Executar testes para validar o código.
5. Validar manualmente a nova funcionalidade.
6. Criar um pull request (PR).
7. Mesclar o código depois da revisão e da aprovação da integração contínua.

> [!NOTE]
> Os detalhes variam de acordo com a equipe e a organização, mas muitos fluxos seguem uma variação dessas etapas.

## Modos de conversa

O **modo de conversa** controla o nível de autonomia do agente. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre os modos:

- **Interactive**: você e o agente trabalham em conjunto. O agente sugere mudanças e aguarda sua orientação antes de continuar.
- **Plan**: o agente cria um plano e não pode editar os arquivos do projeto.
- **Autopilot**: o agente trabalha de forma autônoma, escrevendo código, executando testes e iterando até considerar a tarefa concluída.

Comece no modo Plan, revise o plano e use o modo Autopilot para implementá-lo.

## Comece pela issue

Prepare o Codespace e crie uma branch para o trabalho de filtragem.

1. Retorne ao Codespace. Se ele estiver parado, reinicie-o.
2. Confirme que o pull request da classificação por estrelas foi mesclado.
3. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Atualize `main` e crie uma branch para a filtragem:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicie o Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pressione <kbd>Tab</kbd> duas vezes para abrir a guia **Issues**.
7. Pressione <kbd>A</kbd> para exibir todas as issues.
8. Use as setas para selecionar a issue **Allow users to filter games by category and publisher**.
9. Pressione <kbd>C</kbd> para adicionar a issue ao prompt e retornar à guia **Session**.

O prompt passa a incluir `#7` (ou um número semelhante), que adiciona uma issue ou um pull request do GitHub ao contexto.

## Planeje a filtragem

O planejamento permite definir a abordagem antes de delegar a implementação ao Copilot.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar para o modo Plan e confirme que o indicador abaixo do prompt mostra **Plan**.
2. Depois da referência à issue, envie este prompt:

   ```plaintext
   Create a plan for implementing this feature.
   ```

3. Responda às perguntas do Copilot sobre o comportamento dos filtros com base nas suas preferências.
4. Quando o plano estiver pronto, pressione <kbd>Control</kbd>+<kbd>E</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandi-lo.
5. Revise o plano e peça ao Copilot que altere qualquer parte que não corresponda às decisões tomadas.

## Aprove o modo Autopilot

Depois de revisar o plano, autorize a implementação.

1. Selecione **Accept plan and build on autopilot (recommended)** ou a opção equivalente na versão instalada.
2. Confirme que o indicador abaixo do prompt mostra **Autopilot**.
3. Acompanhe o Copilot enquanto ele implementa o plano e executa os testes.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente. Revise o plano primeiro. Se o Copilot informar que faltam dependências ou que há conflito de porta, resolva o problema de configuração antes de considerar as verificações concluídas.

## Revise e verifique a implementação

Revise o código gerado e os resultados das verificações antes de mesclar qualquer mudança.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar no modo Interactive e confirme que o indicador não mostra **Plan** nem **Autopilot**.
2. Digite `/diff` e inspecione a implementação dos filtros e os testes.
3. Compare o resultado com a issue e com as decisões do planejamento.
4. Depois da revisão, pressione <kbd>Esc</kbd> para sair da tela de diff.
5. Revise os resultados das verificações do projeto e peça ao Copilot que corrija as falhas.

## Explore a funcionalidade

Inicie o aplicativo e abra o site pela porta encaminhada do Codespaces para testar os filtros.

1. Peça ao Copilot para iniciar o aplicativo:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Teste os filtros por categoria e editora e as combinações acordadas no plano.
4. Confirme se a redefinição dos filtros e o comportamento sem resultados correspondem à issue e às decisões tomadas.
5. Retorne ao Codespace e peça ao Copilot que pare o servidor iniciado.

## Resumo e próximos passos

Você usou diferentes modos de conversa para desenvolver e revisar uma funcionalidade. Nesta lição, você:

- iniciou uma conversa do Copilot a partir de uma issue do GitHub.
- definiu os requisitos no modo Plan.
- implementou a funcionalidade no modo Autopilot.
- revisou o código.
- validou a funcionalidade manualmente em um navegador encaminhado.

Em seguida, [use instruções personalizadas][next-lesson] para orientar como o Copilot gera código.

## Recursos

- [Autopilot no GitHub Copilot CLI][autopilot]
- [Referência de comandos do Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
