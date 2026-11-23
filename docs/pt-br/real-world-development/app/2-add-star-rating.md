---
title: "Lição 2 - Adicionar classificações por estrelas: uma melhoria rápida"
description: "Inicie sua primeira sessão de agente no aplicativo GitHub Copilot, faça uma pequena alteração nos cartões dos jogos e mescle-a no seu primeiro pull request."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Agora que você conheceu o espaço de trabalho e usou um chat rápido, chegou a hora de iniciar uma **sessão de agente** e fazer sua primeira mudança no projeto. Você manterá o escopo pequeno: os dados dos jogos já incluem uma classificação por estrelas, mas os cartões da página inicial ainda não a exibem. Você pedirá ao agente que mostre a classificação, revisará a mudança e fará o merge no seu primeiro pull request.

Nesta lição, você:

- iniciará uma sessão de agente e aprenderá como ela é estruturada;
- pedirá ao agente que faça uma pequena mudança focada no projeto;
- revisará a mudança na visualização de diff do espaço de trabalho;
- executará o aplicativo localmente para confirmar a mudança no navegador;
- abrirá e mesclará seu primeiro pull request.

## Cenário

Cada jogo da Tailspin Toys pode ter uma classificação por estrelas, que já aparece na página de detalhes do jogo. No entanto, os cartões da página inicial mostram apenas o título, a categoria, a editora e a descrição. Como exercício inicial, você pedirá ao agente que exiba a classificação existente em cada cartão — uma mudança pequena e independente, ideal para sua primeira sessão.

## Anatomia de uma sessão

Uma **sessão** é uma conversa com um agente executada em seu próprio espaço de trabalho isolado. Cada sessão recebe uma **git worktree e uma branch dedicadas**, o que permite executar várias sessões ao mesmo tempo — uma adicionando um recurso e outra corrigindo um bug — sem que as mudanças entrem em conflito. Suas sessões aparecem na barra lateral, agrupadas por repositório. Selecione uma delas para acessá-la.

Em uma sessão, você verá três elementos: a **conversa** com o agente, a **atividade das ferramentas** do agente enquanto explora e edita arquivos e a lista de **arquivos alterados** com os respectivos diffs.

## Inicie uma sessão e solicite a mudança

Vamos iniciar uma sessão para explorar o projeto e implementar o recurso. Durante a [configuração do aplicativo][prior-lesson], você adicionou o projeto por meio do repositório do GitHub. Criaremos uma sessão para esse repositório e solicitaremos a mudança.

1. Volte ao aplicativo GitHub Copilot ou abra-o.
2. Selecione **+** na parte superior da barra lateral para iniciar uma sessão.
3. Selecione `tailspin-toys` como repositório.
4. Escolha **new working tree** e o modo **Interactive** abaixo da caixa de prompt. Use o prompt a seguir para solicitar a mudança:

    ```plaintext
    Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
    ```

5. Pressione <kbd>Enter</kbd> para enviar o prompt ao Copilot.

O aplicativo Copilot começa criando uma nova worktree, uma cópia isolada do projeto. Em seguida, explora o projeto, localiza os arquivos que precisam ser atualizados para adicionar o recurso e cria o código necessário. Você acabou de adicionar um recurso com o aplicativo Copilot!

## Revise o diff

Todas as mudanças geradas por IA devem ser revisadas antes do merge, mesmo as pequenas. Vamos explorar as mudanças diretamente no aplicativo Copilot.

1. No canto superior direito do aplicativo, selecione **Toggle review panel**. No painel que abrir, selecione **Changes** para ver os arquivos alterados e seus diffs.

    ![Barra de ferramentas superior do aplicativo GitHub Copilot com uma seta apontando para o botão Toggle review panel à direita de Create PR](../../../_images/app-2-review-panel.png)

2. Você deverá ver código adicionado a `GameCard.astro`, o arquivo principal usado para exibir os detalhes do jogo. Ele deve ser semelhante ao seguinte: um pequeno bloco que renderiza a classificação quando disponível e mostra "No rating yet" quando `starRating` é `null`:

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> Como todas as ferramentas de IA generativa, o Copilot é probabilístico, e não determinístico, por isso o código exato pode variar. Ele deverá ser relativamente semelhante ao exemplo.

## Confira as mudanças

Revise os resultados das verificações automatizadas do agente antes de abrir o navegador. Confirme que os testes cobrem um `starRating` numérico e a alternativa para `null`. Um pré-requisito ausente ou uma verificação ignorada não conta como aprovação; revise qualquer solicitação de instalação antes de aprová-la.

Não basta ler o código e presumir que funciona. Vamos pedir ao Copilot que abra o site para que possamos examinar a interface atualizada. Para isso, podemos pedir que ele inicie o site e o abra em um canvas de navegador.

> [!TIP]
> Um canvas é um widget interativo disponível dentro do aplicativo Copilot. Você explorará canvases personalizados e até criará um mais adiante, mas, por enquanto, usaremos o canvas de navegador integrado.

1. Use o prompt a seguir para pedir ao Copilot que inicie o aplicativo e abra a página no canvas de navegador:

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. Em alguns instantes, o aplicativo será iniciado e uma janela do navegador será aberta dentro do aplicativo Copilot.
3. Confirme se os cartões dos jogos com classificação exibem a nota de um total de cinco.
4. Quando terminar, use o prompt a seguir para pedir ao Copilot que interrompa o servidor de desenvolvimento iniciado para esta sessão:

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## Abra e mescle seu primeiro pull request

Você criou o recurso! Agora é hora de criar um pull request (PR) para mesclar o novo código à base existente.

1. Selecione **Create PR** no canto superior direito.
2. Se solicitado, selecione **Sign in with your browser** e siga as instruções para se autenticar.
3. Se o Copilot informar que a política do repositório exige uma issue relacionada antes de criar o PR, confirme que ele deve seguir esse processo.
4. O Copilot começará a criar o PR.
5. Selecione o indicador **PR** logo acima do chat para abrir o pull request no painel de revisão. Faça as revisões necessárias ali.
6. Quando estiver tudo pronto, selecione **Ready to merge**.
7. Na nova caixa de diálogo, selecione **Merge pull request** para mesclar o pull request.

## Resumo e próximos passos

Parabéns! Você entregou sua primeira mudança usando o aplicativo GitHub Copilot. Especificamente, você:

- iniciou uma sessão de agente e aprendeu como as sessões são estruturadas;
- orientou o agente a fazer uma pequena mudança focada nos cartões dos jogos;
- revisou a mudança na visualização de diff do espaço de trabalho;
- executou o aplicativo localmente para confirmar a classificação por estrelas no navegador;
- abriu o PR 1, revisou as verificações e fez o merge explicitamente.

Em seguida, você [começará pela issue de filtragem e usará os modos Plan e Autopilot][next-lesson] para criar um recurso maior.

## Recursos

- [Trabalhar com sessões de agente no aplicativo GitHub Copilot][agent-sessions]
- [Sobre o aplicativo GitHub Copilot][about-copilot-app]
- [Gerenciar issues e pull requests com o aplicativo GitHub Copilot][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#instale-e-configure-o-aplicativo-github-copilot
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
