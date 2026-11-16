---
title: "Lição 10 - Conclusão e próximas etapas"
description: "Recapitule o fluxo de trabalho do Copilot CLI, as duas pull requests, as personalizações reutilizáveis e outros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Você usou o GitHub Copilot CLI em um fluxo de trabalho completo no Tailspin Toys. Você:

- preparou um Codespace, instalou o Copilot CLI, explorou o projeto e encontrou a issue de filtragem inicializada.
- adicionou avaliações em estrelas, revisou o resultado em um navegador encaminhado e fez manualmente o merge da sua primeira pull request (PR).
- começou pela issue de filtragem, definiu a abordagem no modo Plan, implementou no modo Autopilot e revisou no modo Interactive.
- orientou o agente com instruções personalizadas, personalizou a skill `quality-checks` e usou-a para executar testes unitários, lint e verificações de tipos.
- adicionou o servidor Model Context Protocol (MCP) do Playwright e usou-o para explorar a filtragem em um navegador real.
- criou e selecionou um agente personalizado de garantia de qualidade (QA) para avaliar requisitos, cobertura, resultados da skill e evidências do navegador.
- revisou a alteração completa de filtragem e autorizou o Agent Merge para a PR do recurso.
- explorou comandos de barra para contexto, modelos, compartilhamento e delegação opcional na nuvem.

## O que você entregou

O workshop tem dois marcos de PR, cada um em sua própria branch criada a partir de `main` atualizada:

1. **Avaliações em estrelas:** exibir o `starRating` existente e indicar explicitamente quando um card de jogo ainda não tem avaliação.
2. **Filtragem e fluxo de qualidade:** implementar a filtragem, atualizar as instruções e aplicá-las ao recurso, personalizar o relatório `quality-checks`, criar um perfil de QA e incluir os testes relacionados.

Do planejamento da filtragem até a abertura da PR, você usou a mesma conversa e branch. Reunimos esse trabalho em uma única PR para simplificar o workshop.

## Diferentes tipos de verificação

Você verificou o código de várias maneiras: testes automatizados, sua própria inspeção no navegador e a exploração do navegador pelo Copilot por meio do MCP. A skill `quality-checks` executou testes unitários, lint e verificações de tipos e relatou os resultados no novo formato. O QA reuniu esses resultados e os combinou com a revisão dos requisitos e da cobertura de testes antes da PR.

Os testes adicionados devem preencher lacunas reais; uma execução de QA que não precisa de novos testes também pode estar correta. Revise o código e as evidências antes de autorizar o merge e atualize as evidências afetadas depois de qualquer alteração.

## Práticas recomendadas

O contexto e as ferramentas fornecidos ao Copilot moldam o trabalho dele. Neste workshop, você atualizou instruções, personalizou uma skill, criou um perfil de QA e configurou um servidor MCP. Reutilize essas personalizações em diferentes conversas e ajuste-as conforme as necessidades da equipe mudarem. As instruções definem padrões, as skills descrevem tarefas repetíveis, os agentes personalizados definem funções especializadas e os servidores MCP conectam ferramentas externas. Revise as alterações reais e os resultados das ferramentas, não apenas o resumo do agente.

Escolha o **modo e o modelo** de acordo com a tarefa. Use **Plan** para pensar em uma abordagem antes de começar a implementação, **Interactive** para acompanhar de perto alterações focadas e **Autopilot** para tarefas bem delimitadas. Escolha um modelo mais rápido para alterações de rotina e um modelo mais avançado para tarefas complexas.

O contexto continua sendo tão importante quanto a infraestrutura. Descrever com clareza *o que* você quer criar, *por que* e *como* muda significativamente o resultado.

## Mais recursos para explorar

Você concluiu o fluxo de trabalho principal. Confira mais alguns recursos do CLI:

- `/review` para pedir ao agente de revisão de código que analise as alterações.
- `/rubber-duck` para conversar sobre um problema e obter outra perspectiva.
- `/fleet` para coordenar subtarefas independentes em paralelo.
- `/worktree` para isolar uma tarefa separada.
- `/delegate` para enviar uma tarefa ao agente na nuvem do Copilot.

## Próximas etapas

A melhor maneira de melhorar com qualquer ferramenta é continuar usando-a! Use-a em código de produção, em projetos pessoais ou naquele pequeno app que você pensa em criar há anos. Compartilhe o que aprendeu com sua equipe e aprenda com os outros. E, como sempre, explore a documentação.

Se quiser ampliar o Tailspin Toys pelo terminal, continue com a série opcional [Foundry Backer Concierge][foundry]. Para comparar outros ambientes, explore o [workshop do VS Code][vscode], o [workshop do app GitHub Copilot][app] ou o [workshop do agente na nuvem do Copilot][cloud].

## Recursos

- [Sobre o GitHub Copilot CLI][about-cli]
- [Referência de comandos do Copilot CLI][cli-reference]
- [Personalize o Copilot CLI][customize-cli]
- [Gerencie pull requests com o Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
