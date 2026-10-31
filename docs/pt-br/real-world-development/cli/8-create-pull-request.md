---
title: "Lição 8 - Crie e mescle o pull request da funcionalidade"
description: "Revise em conjunto os filtros, as instruções, a skill, o perfil de garantia de qualidade (QA) e os testes; depois, crie um pull request (PR) e use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

A implementação dos filtros, as atualizações das instruções, a personalização da skill, o perfil de garantia de qualidade (QA) e os testes estão salvos em uma branch. Revise tudo em conjunto e abra um pull request. Você mesclou o pull request (PR) da classificação por estrelas manualmente; desta vez, deixe o **Agent Merge** gerenciar o processo.

> [!NOTE]
> Em geral, a funcionalidade, as atualizações das instruções, a skill e o agente de QA ficariam em alguns PRs separados. Para simplificar o workshop, todo o fluxo de filtragem e qualidade está na mesma conversa e branch e será incluído neste PR.

Nesta lição, você vai:

- aprender o que é Agent Merge e como ele automatiza o ciclo de vida da mesclagem.
- inspecionar a mudança completa da funcionalidade e as evidências de verificação.
- criar o PR da filtragem.
- habilitar o Agent Merge somente depois da revisão e confirmar que o PR foi mesclado.

## Cenário

Durante o fluxo de trabalho da filtragem, você usou o Copilot para planejar, implementar e verificar uma funcionalidade. Agora, a Tailspin Toys quer automatizar o trabalho restante do PR, mantendo a autorização de mesclagem sob o controle de quem desenvolve.

## Conheça o Agent Merge

O **Agent Merge** automatiza o trabalho necessário para concluir um pull request. Quando você o habilita, o Copilot resolve os itens que bloqueiam o PR — como falhas nas verificações de integração contínua (CI), comentários de revisão e necessidade de rebase — e habilita a mesclagem automática do GitHub quando o repositório permite.

Até aqui, você selecionou **Merge pull request** manualmente. O Agent Merge pode assumir essa responsabilidade. Revise o trabalho e confirme que ele está pronto antes de habilitar o recurso.

## Use o Agent Merge para gerenciar o PR

Revise todo o código, crie o PR e permita que o Agent Merge gerencie o restante do processo.

1. Retorne ao Codespace.
2. Digite `/agent` para abrir a caixa de diálogo de agentes.
3. Selecione **Default** na lista e pressione <kbd>Enter</kbd>.
4. Crie um PR com o comando `/pr create`.
5. Ative o Agent Merge com `/pr agentmerge`.
6. Aguarde enquanto o Copilot acompanha a integração contínua do PR e, quando tudo for aprovado, realiza a mesclagem.
7. Confirme que o Copilot informa algo semelhante a `PR #14 was squash-merged successfully.`

> [!IMPORTANT]
> O Agent Merge não ignora aprovações obrigatórias, proteção de branch, filas de mesclagem, configurações do repositório nem permissões ausentes. Se o processo for bloqueado, leia o motivo informado e conclua manualmente a mesclagem revisada quando o repositório permitir.

## Resumo e próximos passos

Você automatizou várias etapas do desenvolvimento: geração de código, testes e validação, além do processo de pull request. Nesta lição, você:

- aprendeu o que é Agent Merge e como ele automatiza o ciclo de vida da mesclagem.
- inspecionou a mudança completa da funcionalidade e as evidências de verificação.
- criou o PR da filtragem.
- habilitou o Agent Merge somente depois da revisão e confirmou que o PR foi mesclado.

Em seguida, [explore outros comandos de barra do Copilot CLI][next-lesson] para contexto, modelos, compartilhamento e delegação opcional à nuvem.

## Recursos

- [Gerenciar pull requests com o Copilot CLI][manage-prs]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
