---
title: "Lição 6 - Valide a funcionalidade com Playwright MCP"
description: "Inspecione ou configure o Playwright MCP no Copilot CLI e use-o para explorar os filtros em um navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Além de escrever código, é preciso trabalhar com dados, serviços externos e automações disponíveis ao Copilot. Os servidores MCP estendem os recursos do CLI com ferramentas e serviços.

Nesta lição, você vai:

- entender o que é o Model Context Protocol (MCP) e como o Copilot CLI o usa.
- adicionar o servidor Playwright MCP se ele ainda não estiver disponível.
- pedir ao agente que controle um navegador e explore a funcionalidade de filtragem.

## Cenário

Testes unitários e de ponta a ponta são importantes, mas validar mudanças na interface também exige interagir com ela. Com o Playwright MCP, o Copilot pode usar o site como uma pessoa usuária para verificar se as mudanças funcionam como esperado.

## O que é o Model Context Protocol (MCP)?

O [Model Context Protocol (MCP)][mcp-blog-post] permite que agentes de IA se comuniquem com ferramentas e serviços externos em tempo real. Assim, eles podem acessar informações atualizadas e executar ações em seu nome.

Essas ferramentas e esses recursos são acessados por um servidor MCP, que funciona como uma ponte entre o agente de IA e os serviços externos. Cada servidor MCP oferece um conjunto diferente de ferramentas e recursos:

- O **[servidor MCP do GitHub][github-mcp]** fornece acesso a APIs para gerenciar repositórios, issues e pull requests do GitHub.
- O **[servidor Playwright MCP][playwright-mcp-server]** oferece recursos de automação de navegador com Playwright.

O GitHub mantém um [registro de MCP][mcp-registry] para facilitar a descoberta de servidores e as contribuições ao ecossistema.

> [!CAUTION]
> Trate servidores MCP como qualquer outra dependência do projeto. Antes de usar um, revise o código-fonte, verifique quem o publicou e considere as implicações de segurança.

## Adicione o servidor Playwright MCP

Se o servidor Playwright MCP ainda não estiver disponível, adicione-o para que o Copilot possa interagir com o site.

> [!NOTE]
> Se o Playwright MCP já estiver configurado no Copilot CLI, ignore estas etapas e continue na próxima seção.

1. Retorne ao Codespace.
2. No Copilot CLI, abra a caixa de diálogo de adição de servidor MCP:

   ```plaintext
   /mcp add
   ```

3. Digite `playwright` como nome e pressione <kbd>Tab</kbd>.
4. Confirme o tipo de servidor **STDIO** pressionando <kbd>Enter</kbd> e, em seguida, pressione <kbd>Tab</kbd>.
5. Na caixa de diálogo **Command**, cole:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pressione <kbd>Control</kbd>+<kbd>S</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para salvar o novo servidor MCP.
7. Pressione <kbd>Esc</kbd> para sair da caixa de diálogo MCP.

## Peça ao Copilot para explorar a funcionalidade

Use o Playwright MCP para repetir no navegador a validação manual da funcionalidade.

1. Envie este prompt para pedir ao Copilot que valide a funcionalidade com o servidor Playwright MCP:

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Normalmente, o Copilot identifica sozinho qual servidor MCP deve usar. Indicar o Playwright MCP no prompt ajuda a orientar a tarefa e torna os resultados mais consistentes.

2. Acompanhe as etapas que o Copilot executa no navegador.
3. Leia o relatório e confirme se o comportamento está correto.

O Copilot iniciará o servidor, usará o Playwright para interagir com o site, encerrará o servidor e apresentará um relatório.

## Resumo e próximos passos

Você usou o servidor Playwright MCP para explorar a funcionalidade em um navegador real a partir do Copilot CLI. Nesta lição, você:

- entendeu o que é o Model Context Protocol (MCP) e como o Copilot CLI o usa.
- adicionou o servidor Playwright MCP se ele ainda não estava disponível.
- pediu ao agente que controlasse um navegador e explorasse a funcionalidade de filtragem.

Em seguida, [crie um agente personalizado de QA][next-lesson] que combine a skill e as ferramentas de navegador.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
- [Servidor Playwright MCP da Microsoft][playwright-mcp-server]
- [Adicionar servidores MCP ao Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
