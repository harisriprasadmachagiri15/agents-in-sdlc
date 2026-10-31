---
slug: pt-br/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

O **[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** permite que o GitHub Copilot trabalhe de forma assíncrona na nuvem. Você atribui o trabalho no GitHub, e o cloud agent o executa em segundo plano: explora o repositório, faz alterações e abre um pull request, enquanto você pode se dedicar a outras tarefas.

Ao longo destes exercícios, você adicionará instruções personalizadas que o cloud agent seguirá e, em seguida, atribuirá uma issue do GitHub para que ele realize o trabalho. Você revisará e usará agentes personalizados para orientar a abordagem, monitorará e orientará as sessões no painel de agentes e finalizará revisando os pull requests e iterando sobre os resultados.

## Exercícios

| Exercício | Tema | Descrição |
|----------|-------|-------------|
| [0. Pré-requisitos][ex0] | Configuração | Crie seu repositório e seu codespace |
| [1. Instruções personalizadas][ex1] | Contexto | Adicione as instruções personalizadas que o cloud agent seguirá |
| [2. Cloud agent][ex2] | Agente assíncrono | Atribua issues ao Copilot cloud agent |
| [3. Agentes personalizados][ex3] | Agentes especializados | Revise e use agentes personalizados |
| [4. Gerenciar agentes][ex4] | Monitoramento | Monitore e oriente sessões de agentes |
| [5. Iterar][ex5] | Revisão | Revise PRs, aprimore o trabalho do Copilot e escolha as próximas etapas |

## Pré-requisitos

Antes de participar deste workshop, certifique-se de ter:

- [ ] Uma conta do GitHub com um plano **Copilot Student, Pro, Pro+, Business ou Enterprise** ativo
- [ ] O Copilot cloud agent habilitado para sua conta ou organização

> [!TIP]
> Não tem um plano pago? Estudantes verificados podem obter o GitHub Copilot gratuitamente por meio do [GitHub Education][callout-student-plan-education]. O plano **Copilot Student** inclui os recursos de agente, Model Context Protocol (MCP), revisão de código e a interface de linha de comando (CLI) do GitHub Copilot, conhecida como Copilot CLI, usada neste workshop. Assim, você pode concluir todos os fluxos com ele.

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> O cloud agent está disponível nos planos **Copilot Student, Pro, Pro+, Business e Enterprise** — em todos os planos, exceto o Copilot Free. No Copilot Business ou Enterprise, um administrador precisa habilitar o recurso para a organização.

> [!NOTE]
> Este fluxo não aborda o MCP. Para explorar o uso de servidores MCP com o Copilot, consulte o [fluxo do CLI](../cli/) ou o [fluxo do VS Code](../vscode/).

## Começar

**[Comece pelo Exercício 0: Pré-requisitos →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
