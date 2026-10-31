---
title: "Lição 4 - Oriente o Copilot com instruções personalizadas"
description: "Explore as instruções do repositório, adicione um padrão de documentação e aplique-o ao código de filtragem."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisa ser feita de determinada maneira, disponibilize essa orientação ao Copilot. Os [arquivos de instruções][instruction-files] descrevem não apenas o que você quer, mas também como o código deve ser estruturado. Agora que você implementou os filtros, explore as instruções usadas pelo Copilot, adicione um padrão de documentação e aplique-o ao código.

Nesta lição, você vai:

- explorar como as instruções do repositório e os arquivos de instruções específicos por caminho chegam ao agente.
- atualizar o arquivo de instruções para garantir que os padrões de código sejam seguidos.
- observar o impacto dos arquivos de instruções no código.

## Cenário

A Tailspin Toys tem diretrizes para as práticas de desenvolvimento:

- Comentários devem explicar a intenção e decisões não óbvias, em vez de repetir o código.
- Funções exportadas em `db/` e `src/lib/` devem documentar sua finalidade, parâmetros e valores de retorno com TSDoc/JSDoc, incluindo um argumento `db` injetável quando houver.
- Componentes Astro reutilizáveis devem documentar seus contratos `Props`, e os comentários devem continuar atualizados quando o código relacionado mudar.
- As orientações existentes de formatação e lint devem ser preservadas.

Use arquivos de instruções para orientar o Copilot de acordo com essas práticas.

## Arquivos de instruções

As instruções personalizadas fornecem contexto e preferências ao Copilot. Elas podem especificar convenções de código, bibliotecas e comentários desejados, tanto para todo o repositório quanto para tipos específicos de arquivos.

- `.github/copilot-instructions.md` é o arquivo único enviado ao Copilot em todas as solicitações do repositório. Use-o para informações relevantes à maioria das tarefas.
- `.github/instructions/*.instructions.md` contém orientações para linguagens, tipos de arquivo ou tarefas específicas.

> [!NOTE]
> O suporte a outros formatos de instrução varia entre ambientes. Consulte a [referência de suporte a instruções personalizadas][custom-instructions-support] antes de depender de um formato específico.

## Explore os arquivos de instruções do projeto

O projeto inicial já inclui arquivos de instruções. Examine-os antes de fazer mudanças para entender como eles orientam o Copilot.

1. Retorne ao Codespace.
2. No editor do Codespaces, não no terminal, abra `.github/copilot-instructions.md`.
3. Observe a descrição do projeto e as orientações de código, que se aplicam a todas as interações com o Copilot neste repositório.
4. Explore os arquivos em `.github/instructions`. Há instruções para Astro, a camada de dados Drizzle, testes e outros tópicos.
5. Abra `.github/instructions/unit-tests.instructions.md` e observe o campo `applyTo`, que define os arquivos aos quais as instruções se aplicam.
6. Abra `.github/instructions/drizzle.instructions.md` e observe os links para outros arquivos de instruções e arquivos do projeto. Esses links permitem dividir orientações maiores em arquivos reutilizáveis menores e apontar exemplos.

## Atualize as instruções da equipe

Adicione ao arquivo principal uma orientação para que o código TypeScript novo inclua comentários TSDoc.

1. Em `.github/copilot-instructions.md`, localize a seção **Code formatting guidance**, que deve estar perto da linha 35.
2. Adicione este item ao final da seção:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

O arquivo será salvo automaticamente.

## Use as instruções atualizadas

O Copilot CLI carrega as instruções do repositório quando uma conversa começa. Retome a conversa de filtragem para aplicar a orientação nova sem perder o contexto da funcionalidade.

1. Se a conversa de filtragem ainda estiver aberta, digite `/exit`. O Copilot CLI salva as conversas automaticamente.
2. Na raiz do repositório e na branch `game-filters-cli`, retome a conversa salva:

   ```bash
   copilot --resume
   ```

   Se solicitado, selecione a conversa de filtragem e confirme que a branch `game-filters-cli` está em uso.
3. Peça ao Copilot que aplique a orientação atualizada:

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. Digite `/diff` e revise os arquivos TypeScript alterados. Confirme que os comentários TSDoc explicam o código corretamente.

## Resumo e próximos passos

Você explorou como o Copilot usa os arquivos de instruções e aplicou um novo padrão à funcionalidade. Nesta lição, você:

- explorou como as instruções do repositório e os arquivos de instruções específicos por caminho chegam ao agente.
- atualizou o arquivo de instruções para garantir que os padrões de código sejam seguidos.
- observou o impacto dos arquivos de instruções no código.

Em seguida, [personalize e execute a skill reutilizável `quality-checks`][next-lesson] para padronizar a execução de lint e testes.

## Recursos

- [Adicionar instruções personalizadas ao Copilot CLI][instruction-files]
- [Suporte a instruções personalizadas][custom-instructions-support]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [Awesome Copilot: coleção de instruções e outros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
