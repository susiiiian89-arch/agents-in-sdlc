---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos últimos exercícios, você pediu ao agente na nuvem do Copilot que executasse três tarefas para melhorar a experiência do usuário e adicionar funcionalidades. Embora o agente na nuvem trabalhe de forma assíncrona e autônoma, ainda é importante acompanhar essas tarefas.

Há várias ferramentas para gerenciar as tarefas atribuídas ao agente na nuvem, incluindo a [página de agentes][agents-page] no GitHub.com. Nesse painel central, você pode ver todas as tarefas dos agentes que têm pull requests (PRs) abertas. Também pode explorar as operações realizadas e orientar uma sessão em andamento.

Neste exercício, você vai:

- explorar a página de agentes para acompanhar as tarefas do agente na nuvem.
- orientar uma sessão em andamento para solicitar funcionalidade adicional.

## Cenário

Depois que o agente recebeu a tarefa de criar um modo de alto contraste, a equipe percebeu que também seria um bom momento para adicionar um modo claro. Como o trabalho já envolvia atualizar o estilo do site e adicionar controles, pareceu lógico incluir essa funcionalidade. Você orientará o trabalho do agente para que ele adicione também um modo claro.

## Revise as tarefas do agente na nuvem do Copilot

Vamos verificar o status atual de todas as tarefas atribuídas ao agente na nuvem do Copilot.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe a lista de tarefas no painel principal e no painel à esquerda. Você deverá ver as tarefas atribuídas ao Copilot, incluindo:
   - atualizar a documentação da base de código.
   - gerar APIs para modificar jogos.
   - adicionar um modo de alto contraste ao site.
3. Selecione uma das tarefas em andamento. Revise as operações já executadas pelo Copilot, que podem incluir:
   - clonar o código do repositório.
   - criar o ambiente de trabalho do Copilot.
   - configurar servidores MCP.
   - executar várias etapas para concluir a tarefa atribuída.

> [!NOTE]
> As etapas listadas variam de acordo com o estado do trabalho do Copilot e a abordagem que ele usou.

4. Observe também o painel de pull requests que aparece à direita. Ele permite acompanhar a PR e os arquivos alterados.

## Oriente o agente na nuvem

Agora que você viu as tarefas ativas, vamos pedir ao Copilot que adicione o controle do modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão relacionada à criação do modo de alto contraste. O título exato varia conforme o nome usado pelo Copilot e o estado atual do trabalho.

   ![Sessão de acessibilidade no painel central de agentes](../../../_images/ex6-accessibility-session.png)

2. Acompanhe a sessão por alguns minutos, até que ela indique que a configuração foi concluída e o trabalho começou. Isso ficará evidente quando você começar a ver mensagens semelhantes às mostradas abaixo.
3. Na caixa de diálogo **Steer active session while Copilot is working**, adicione o seguinte prompt:

   ```plaintext
   Enquanto trabalhamos no modo de alto contraste, vamos também adicionar um modo claro. Deve haver um controle para esse modo, para que os usuários possam selecionar o modo de exibição desejado.
   ```

   ![Tarefa do agente na nuvem na página de agentes, com a caixa de diálogo para orientar uma sessão ativa do Copilot destacada](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe como o Copilot confirma o recebimento do prompt e o inclui no fluxo de trabalho.

## Deixe o Copilot trabalhar

Como antes, o Copilot começará a trabalhar na tarefa atualizada. Ele incorporará a nova solicitação ao fluxo depois de concluir a etapa em andamento quando você enviou a mensagem.

Como isso levará alguns minutos, aproveite para fazer uma pausa e refletir sobre o que aprendeu e explorou até agora.

## Resumo e próximas etapas

Este exercício apresentou a página de agentes do Copilot, o ponto central para acompanhar e orientar as tarefas do agente na nuvem do GitHub Copilot. Nela, você pode acompanhar tarefas ativas e concluídas, revisar o trabalho e redirecionar tarefas em andamento para ajustar o escopo ou fornecer orientações adicionais.

Neste exercício, você explorou:

- como usar o painel central e a página de agentes para acompanhar tarefas do agente na nuvem.
- como redirecionar uma sessão em andamento para solicitar funcionalidade adicional.

Enquanto o Copilot conclui o trabalho nos recursos de acessibilidade, vamos [iterar sobre as pull requests criadas pelo Copilot][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre os agentes personalizados][custom-agents]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
