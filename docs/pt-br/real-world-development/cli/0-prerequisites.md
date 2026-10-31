---
title: "Lição 0 - Pré-requisitos"
description: "Crie sua cópia do Tailspin Toys e prepare um Codespace para o workshop do Copilot CLI."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Antes de começar as lições do Copilot CLI, prepare o ambiente. Você criará uma cópia própria do repositório Tailspin Toys e iniciará um [Codespace][codespaces]. No próximo exercício, use o terminal integrado para instalar e executar o Copilot CLI.

Nesta lição, você vai:

- criar sua própria cópia do projeto Tailspin Toys a partir do template.
- criar um Codespace e confirmar que o projeto está pronto.

## Configure o repositório do workshop

Você trabalhará na sua cópia do projeto Tailspin Toys. Crie-a agora a partir do [repositório de template][tailspin-template]. O novo repositório contém todos os arquivos necessários para o workshop.

1. Em uma nova janela do navegador, acesse o [template Tailspin Toys][tailspin-template].
2. Crie sua cópia selecionando **Use this template** e, em seguida, **Create a new repository**.
3. Se estiver participando de um evento conduzido pelo GitHub ou pela Microsoft, siga as instruções das pessoas mentoras. Caso contrário, crie o repositório em uma organização na qual você tenha acesso ao GitHub Copilot.
4. Anote o caminho do repositório criado (`organization-or-user-name/repository-name`), que será usado mais adiante no workshop.

> [!NOTE]
> Ao criar o repositório a partir do template, um backlog de issues do GitHub é criado automaticamente. Você usará essas issues no workshop; não precisa criar nenhuma.

Use uma cópia recente do template do workshop. Ela inclui instruções do repositório, o código do aplicativo, testes, a skill `quality-checks` e o backlog necessário. Se usar uma cópia antiga, confirme com a pessoa que facilita o workshop se ela contém os arquivos necessários.

## Crie um Codespace

Use um Codespace para concluir o workshop.

[GitHub Codespaces][codespaces] é um ambiente de desenvolvimento na nuvem que permite escrever, executar e depurar código diretamente no navegador. Ele oferece um editor completo com suporte a várias linguagens, extensões e ferramentas.

1. Acesse o repositório que você criou.
2. Selecione **Code**.
3. Selecione a guia **Codespaces** e, em seguida, **Create codespace on main**.
4. Aguarde a conclusão da configuração. O template instala as dependências do projeto, o Chromium do Playwright e o banco de dados local.
5. Se aparecer **Do you trust the authors of the files in this folder?**, selecione **Trust Folder & Continue**.
6. Abra um terminal na raiz do repositório e inicie o aplicativo:

   ```bash
   npm run dev
   ```

7. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser** e confirme que o site Tailspin Toys carrega.
8. Retorne ao terminal e pare o servidor de desenvolvimento com <kbd>Ctrl</kbd>+<kbd>C</kbd>.

## Resumo e próximos passos

O ambiente está pronto. Nesta lição, você:

- criou sua própria cópia do projeto Tailspin Toys a partir do template.
- criou um Codespace e confirmou que o projeto estava pronto.

Em seguida, [instale o GitHub Copilot CLI][next-lesson] no Codespace e autentique-se com sua conta do GitHub.

## Recursos

- [Visão geral do GitHub Codespaces][codespaces]
- [Criar um repositório a partir de um template][template-repository]
- [Guia de início rápido do Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-install-copilot-cli/
[codespaces]: https://github.com/features/codespaces
