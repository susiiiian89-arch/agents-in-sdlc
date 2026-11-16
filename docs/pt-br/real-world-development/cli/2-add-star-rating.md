---
title: "Lição 2 - Adicione avaliações em estrelas: uma vitória rápida"
description: "Use o Copilot CLI para fazer uma pequena alteração nos cards de jogos, revise-a em um navegador encaminhado e faça o merge como sua primeira pull request."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Agora que você instalou o Copilot CLI e experimentou uma conversa, é hora de fazer sua primeira alteração no projeto. Você vai começar com algo pequeno: os jogos já têm uma avaliação em estrelas nos dados, mas os cards da página inicial ainda não a exibem. Você pedirá ao agente que mostre essa informação, revisará a alteração e fará o merge como sua primeira pull request.

Nesta lição, você vai:

- iniciar uma conversa focada do Copilot em uma branch de funcionalidade.
- pedir ao agente para fazer uma pequena alteração no projeto.
- revisar a alteração com `/diff`.
- executar o app para confirmar a alteração em um navegador encaminhado.
- abrir e fazer o merge da sua primeira pull request.

## Cenário

Cada jogo no Tailspin Toys pode ter uma avaliação em estrelas, que já aparece na página de detalhes do jogo. Os cards da página inicial, porém, mostram apenas o título, a categoria, a editora e a descrição. Como aquecimento, você pedirá ao agente que exiba a avaliação existente em cada card: uma alteração pequena e independente, ideal para sua primeira sessão.

## Anatomia de uma conversa

Uma **conversa** é onde você trabalha com o Copilot CLI em uma tarefa. Diferentemente do app do Copilot, uma conversa normal do CLI usa o repositório e a branch do Git atualmente selecionados no terminal, em vez de criar um worktree dedicado. As conversas salvas permitem retomar a mesma discussão mais tarde, enquanto os arquivos e a branch continuam sendo o estado comum do Git no disco.

Em uma conversa, você verá três coisas: seus prompts e as respostas do agente, a atividade das ferramentas do agente enquanto ele explora e edita arquivos e as alterações que você pode inspecionar com `/diff`.

## Inicie uma conversa e solicite a alteração

Vamos iniciar uma nova conversa para começar a implementar o recurso.

1. Volte ao seu Codespace.
2. Se o terminal não estiver aberto como antes, pressione <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Se o Copilot ainda não estiver aberto, inicie-o com o seguinte comando:

   ```bash
   copilot --yolo
   ```

4. Inicie uma nova sessão com o comando de barra `/new` e pressione <kbd>Enter</kbd>.
5. Use o seguinte prompt para solicitar a alteração:

   ```plaintext
   Exiba o starRating de cada jogo de 0 a 5 nos cards da lista. Se a avaliação for null, mostre "No rating yet". Mantenha o layout dos cards, adicione testes e execute as verificações relevantes.
   ```

O Copilot explora o projeto, localiza os arquivos usados para exibir os detalhes dos jogos e cria o código necessário. Você acabou de adicionar um recurso com o Copilot CLI!

## Revise o diff

Todas as alterações geradas por IA devem ser revisadas antes do merge, mesmo as pequenas. Vamos explorar as alterações aqui mesmo no Copilot CLI.

1. Digite `/diff` e inspecione cada arquivo alterado.
2. Confirme se o card do jogo exibe a avaliação numérica quando ela está disponível e `No rating yet` quando `starRating` é `null`.
3. Confirme se os testes cobrem os dois estados.
4. Revise os resultados das verificações executadas pelo Copilot e peça que ele corrija qualquer falha.
5. Quando terminar a revisão, pressione <kbd>Esc</kbd> para sair da tela de diff.

> [!NOTE]
> Como todas as ferramentas de IA generativa, o Copilot é probabilístico, não determinístico, então seu código exato pode variar. Revise o comportamento em vez de esperar uma implementação específica.

## Confira as alterações

Não devemos apenas ler o código e presumir que funciona. Vamos pedir ao Copilot que inicie o site para examinarmos a interface atualizada no navegador encaminhado pelo Codespaces.

1. Peça ao Copilot para iniciar o app:

   ```plaintext
   Inicie o app para que eu possa verificar no navegador a alteração da avaliação em estrelas. Informe a URL e mantenha o servidor em execução.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Confirme se os cards dos jogos exibem suas avaliações de zero a cinco.
4. Volte ao Copilot e peça que pare o servidor iniciado:

   ```plaintext
   Pare o servidor de desenvolvimento que você iniciou.
   ```

## Abra e faça o merge da sua primeira pull request

Você criou o recurso! Agora é hora de criar uma pull request (PR) para fazer o merge do novo código no projeto.

1. Peça ao agente padrão que faça o commit da alteração:

   ```plaintext
   Faça commit das alterações de avaliação em estrelas revisadas com uma mensagem de commit adequada.
   ```

2. Digite `/pr create`. O Copilot CLI enviará o commit existente ao criar a PR e exibirá o URL dela.
3. Abra a PR mantendo pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando o URL exibido pelo Copilot CLI.
4. Revise os arquivos alterados e as verificações.
5. Quando estiver tudo pronto, selecione **Merge pull request** e confirme o merge.
6. Volte ao seu Codespace e saia do Copilot CLI com `/exit`.
7. Atualize sua `main` local:

   ```bash
   git checkout main
   git pull
   ```

## Resumo e próximas etapas

Parabéns! Você entregou sua primeira alteração usando o GitHub Copilot CLI. Em especial, você:

- iniciou uma conversa focada do Copilot em uma branch de funcionalidade.
- orientou o agente para fazer uma pequena alteração nos cards de jogos.
- revisou a alteração com `/diff`.
- executou o app para confirmar a avaliação em estrelas em um navegador encaminhado.
- abriu e fez o merge da sua primeira pull request.

Na próxima lição, você [começará pela issue de filtragem e usará os modos Plan e Autopilot][next-lesson] para criar um recurso maior.

## Recursos

- [Sobre o GitHub Copilot CLI][about-copilot-cli]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
