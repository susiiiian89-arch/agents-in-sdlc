---
title: "Lição 8 - Crie e faça o merge da PR do recurso"
description: "Revise a filtragem, as instruções, a atualização da skill, o perfil de QA e os testes em conjunto; depois, crie uma PR e use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Sua implementação de filtragem, as atualizações das instruções, a atualização da skill, o perfil de garantia de qualidade (QA) e os testes estão salvos em uma única branch. Agora é hora de revisá-los em conjunto e abrir uma pull request. Você fez o merge da pull request (PR) de avaliação em estrelas manualmente; desta vez, deixará que o **Agent Merge** gerencie o processo.

> [!NOTE]
> Normalmente, dividiríamos o recurso, as atualizações de instruções, a atualização da skill e o agente de QA em algumas PRs separadas. Para simplificar o workshop, mantivemos todo o fluxo de filtragem e qualidade em uma conversa e branch, e todo esse trabalho fará parte desta PR.

Nesta lição, você vai:

- aprender o que é o Agent Merge e como ele automatiza o ciclo de vida do merge.
- inspecionar a alteração completa do recurso e as evidências de verificação.
- criar a PR de filtragem.
- habilitar o Agent Merge somente depois da revisão e confirmar se a PR foi mesclada.

## Cenário

Ao longo do fluxo de trabalho de filtragem, você usou o Copilot para planejar, implementar e verificar um recurso. Agora, a Tailspin Toys quer automatizar o trabalho restante da PR, mantendo a autorização de merge sob o controle do desenvolvedor.

## Conheça o Agent Merge

O **Agent Merge** automatiza o trabalho restante necessário para concluir uma pull request. Quando você o habilita, o Copilot resolve os impedimentos da PR — corrige verificações de integração contínua (CI) com falha, responde aos comentários de revisão e faz rebase quando necessário — e, em seguida, habilita o merge automático do GitHub quando o repositório permite.

Até agora, você selecionou **Merge pull request** manualmente. O Agent Merge pode assumir essa responsabilidade. Revise o trabalho e confirme que ele está pronto antes de habilitar o Agent Merge.

## Use o Agent Merge para gerenciar a PR

Com todo o código criado, vamos revisá-lo em conjunto, criar a PR e deixar que o Agent Merge gerencie o restante do processo.

1. Volte ao seu Codespace.
2. Abra a caixa de diálogo do agente digitando `/agent`.
3. Selecione **Default** na lista de opções e pressione <kbd>Enter</kbd>.
4. Crie uma nova PR com o comando `/pr create`.
5. Ative o Agent Merge com `/pr agentmerge`.
6. O Copilot acompanhará o processo de integração contínua da PR. Quando tudo for aprovado, ele fará o merge.
7. Confirme se uma mensagem do Copilot informa algo semelhante a "PR #14 was squash-merged successfully."

> [!IMPORTANT]
> O Agent Merge não ignora aprovações obrigatórias, proteção de branch, filas de merge, configurações do repositório nem permissões ausentes. Se o processo for bloqueado, leia o motivo informado e conclua manualmente o merge revisado quando o repositório permitir.

## Resumo e próximas etapas

Você automatizou várias etapas do processo de desenvolvimento: geração de código, testes, validação e, agora, o processo da pull request. Você:

- aprendeu o que é o Agent Merge e como ele automatiza o ciclo de vida do merge.
- inspecionou a alteração completa do recurso e as evidências de verificação.
- criou a PR de filtragem.
- habilitou o Agent Merge somente depois da revisão e confirmou se a PR foi mesclada.

Na próxima lição, você [explorará outros comandos de barra úteis do Copilot CLI][next-lesson] para contexto, modelos, compartilhamento e delegação opcional na nuvem.

## Recursos

- [Gerencie pull requests com o Copilot CLI][manage-prs]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
