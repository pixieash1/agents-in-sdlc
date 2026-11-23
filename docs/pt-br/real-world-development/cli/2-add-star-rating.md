---
title: "Lição 2 - Adicionar classificações por estrelas: uma melhoria rápida"
description: "Use o Copilot CLI para fazer uma pequena mudança nos cartões de jogos, revise-a no navegador encaminhado e mescle-a no seu primeiro pull request."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Agora que você instalou o Copilot CLI e iniciou uma conversa, chegou a hora de fazer sua primeira mudança no projeto. Você manterá o escopo pequeno: os dados dos jogos já incluem uma classificação por estrelas, mas os cartões da página inicial ainda não a exibem. Você pedirá ao agente que mostre a classificação, revisará a mudança e fará o merge no seu primeiro pull request.

Nesta lição, você:

- iniciará uma conversa focada do Copilot em uma branch de recurso;
- pedirá ao agente que faça uma pequena mudança no projeto;
- revisará a mudança com `/diff`;
- executará o aplicativo para confirmar a mudança em um navegador encaminhado;
- abrirá e mesclará seu primeiro pull request.

## Cenário

Cada jogo da Tailspin Toys pode ter uma classificação por estrelas, que já aparece na página de detalhes do jogo. No entanto, os cartões da página inicial mostram apenas o título, a categoria, a editora e a descrição. Como exercício inicial, você pedirá ao agente que exiba a classificação existente em cada cartão — uma mudança pequena e independente, ideal para sua primeira sessão.

## Anatomia de uma conversa

Uma **conversa** é onde você trabalha com o Copilot CLI em uma tarefa. Diferentemente do aplicativo Copilot, uma conversa normal da CLI usa o repositório e a branch atualmente selecionados no terminal, em vez de criar uma worktree dedicada. As conversas salvas permitem retomar a mesma discussão mais tarde, enquanto os arquivos e a branch continuam sendo o estado comum do Git no disco.

Em uma conversa, você verá três coisas: seus prompts e as respostas do agente; a atividade das ferramentas que o agente usa para explorar e editar arquivos; e as mudanças que você pode inspecionar com `/diff`.

## Inicie uma conversa e solicite a mudança

Vamos iniciar uma nova conversa para implementar o recurso.

1. Volte ao seu Codespace.
2. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Se ainda não estiver em execução, inicie o Copilot com o comando a seguir:

   ```bash
   copilot --yolo
   ```

4. Inicie uma nova sessão com o comando slash `/new` e pressione <kbd>Enter</kbd>.
5. Use este prompt para solicitar a mudança:

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

O Copilot explorará o projeto, localizará os arquivos usados para exibir os detalhes dos jogos e criará o código necessário. Você acabou de adicionar um recurso com o Copilot CLI!

## Revise o diff

Toda mudança gerada por IA deve ser revisada antes do merge, mesmo quando for pequena. Vamos explorar as mudanças diretamente no Copilot CLI.

1. Digite `/diff` e inspecione cada arquivo alterado.
2. Confirme que o cartão do jogo mostra a classificação numérica quando ela está disponível e `No rating yet` quando `starRating` é `null`.
3. Confirme que os testes cobrem os dois estados.
4. Revise os resultados das verificações executadas pelo Copilot e peça que ele corrija eventuais falhas.
5. Quando terminar a revisão, pressione <kbd>Esc</kbd> para sair da tela de diff.

> [!NOTE]
> Como todas as ferramentas de IA generativa, o Copilot é probabilístico, e não determinístico, por isso o código exato pode variar. Revise o comportamento em vez de esperar uma implementação idêntica.

## Confira as mudanças

Não basta ler o código e presumir que funciona. Vamos pedir ao Copilot que inicie o site para que possamos examinar a interface atualizada no navegador encaminhado pelo Codespaces.

1. Peça ao Copilot que inicie o aplicativo:

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Confirme que os cartões dos jogos mostram as classificações de zero a cinco.
4. Volte ao Copilot e peça que interrompa o servidor iniciado:

   ```plaintext
   Stop the development server you started.
   ```

## Abra e mescle seu primeiro pull request

Você criou o recurso! Agora é hora de abrir um pull request (PR) para mesclar o novo código ao projeto.

1. Peça ao agente padrão que faça commit da mudança:

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. Digite `/pr create`. O Copilot CLI enviará o commit existente ao criar o PR e exibirá o URL do PR.
3. Abra o PR mantendo pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando o URL exibido pelo Copilot CLI.
4. Revise os arquivos alterados e as verificações.
5. Quando estiver tudo pronto, selecione **Merge pull request** e confirme o merge.
6. Volte ao Codespace e saia do Copilot CLI com `/exit`.
7. Atualize sua `main` local:

   ```bash
   git checkout main
   git pull
   ```

## Resumo e próximos passos

Parabéns! Você entregou sua primeira mudança com o GitHub Copilot CLI. Especificamente, você:

- iniciou uma conversa focada do Copilot em uma branch de recurso;
- orientou o agente a fazer uma pequena mudança nos cartões dos jogos;
- revisou a mudança com `/diff`;
- executou o aplicativo para confirmar a classificação por estrelas em um navegador encaminhado;
- abriu e mesclou seu primeiro pull request.

Em seguida, você [começará pela issue de filtragem e usará os modos Plan e Autopilot][next-lesson] para criar um recurso maior.

## Recursos

- [Sobre o GitHub Copilot CLI][about-copilot-cli]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
