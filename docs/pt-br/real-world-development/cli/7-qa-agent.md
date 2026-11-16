---
title: "Lição 7 - Crie e use um agente de QA"
description: "Crie um agente personalizado de QA que combine os requisitos da issue, a skill quality-checks e o Playwright MCP."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Você usou a skill `quality-checks` para executar verificações automatizadas e o Playwright MCP para observar a experiência de filtragem em um navegador. Agora, você reunirá esses recursos em um agente personalizado com um processo de QA bem definido.

Nesta lição, você vai:

- explorar como um agente personalizado trabalha com instruções, skills e ferramentas MCP.
- criar e inspecionar um perfil reutilizável de garantia de qualidade.
- selecionar o agente de QA e revisar as descobertas dele em relação à issue de filtragem.

## Cenário

A Tailspin Toys quer revisar de maneira consistente os requisitos, a qualidade do código, as verificações automatizadas, a cobertura de testes e o comportamento no navegador antes de abrir uma pull request (PR). Um agente personalizado pode coordenar esse processo de QA e fornecer um relatório reutilizável.

## O que é um agente personalizado?

Um agente personalizado é uma versão especializada do Copilot, definida em um perfil Markdown. O perfil descreve a finalidade, as instruções e as ferramentas disponíveis para o agente. Neste workshop, você definirá a função de QA em `.github/agents/qa.agent.md` e a selecionará no Copilot CLI.

As personalizações usadas até agora têm funções diferentes. As instruções do repositório descrevem os padrões da equipe. A skill `quality-checks` reúne verificações repetíveis. O Playwright MCP fornece ferramentas de navegador. O perfil de QA orienta o Copilot sobre como usar esses recursos para avaliar os requisitos e relatar as descobertas. Ele não substitui os outros recursos nem exige outra conversa.

## Crie o perfil de QA

Antes de abrir a PR do recurso, peça ao Copilot que crie um perfil de QA reutilizável. O perfil definirá tanto as verificações de QA quanto os limites que o agente deve respeitar.

1. Volte ao seu Codespace e confirme se a conversa de filtragem está no modo Interactive.
2. Peça ao agente padrão para criar o novo agente personalizado:

   ```plaintext
   Crie um agente personalizado chamado QA em .github/agents/qa.agent.md. Ele deve verificar os recursos com base nas issues e nos requisitos acordados, seguir as instruções do repositório, executar a skill quality-checks, usar o Playwright MCP para verificar o comportamento e adicionar testes quando a cobertura estiver incompleta.

   Faça com que ele informe cada requisito como aprovado, reprovado ou bloqueado, com as evidências correspondentes. Ele deve pedir autorização antes de alterar o código de implementação e não deve fazer commit nem abrir pull requests.

   Por enquanto, apenas crie o perfil para que eu possa revisá-lo.
   ```

## Inspecione o perfil

Antes de usar o novo agente, revise o perfil para confirmar se o Copilot registrou o fluxo de trabalho e os limites de QA esperados. Isso evita um agente incompleto ou amplo demais fazendo alterações no recurso quando você só quer verificá-lo.

1. Digite `/diff` e abra `.github/agents/qa.agent.md`.
2. Leia o frontmatter. `description` é obrigatório; `name` é opcional, mas incluí-lo dá ao agente um nome de exibição claro.
3. Leia as instruções do perfil e confirme se o QA começa pelos requisitos, segue as instruções do repositório, executa a skill `quality-checks` e usa o Playwright MCP.
4. Confirme se o QA apresenta evidências, pede autorização antes de alterar o código de implementação e não faz commits nem abre pull requests.
5. Se o perfil gerado não contemplar alguma dessas responsabilidades ou algum desses limites, peça ao agente padrão que o revise antes de continuar.

## Execute o QA com base na issue

O Copilot CLI carrega os agentes do projeto quando uma conversa é iniciada. Retome a mesma conversa de filtragem depois de criar o perfil e, em seguida, selecione QA para que o agente possa usar a issue e as decisões de planejamento já disponíveis no contexto.

1. Ative o agente com o seguinte prompt:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como você acabou de criar o agente, talvez ele ainda não apareça na lista de agentes disponíveis. Mesmo assim, o comando acima o ativará.

2. Use o seguinte prompt para pedir que o agente de QA revise o recurso:

   ```plaintext
   Revise o recurso de filtragem com base na issue e nas decisões do nosso plano. Ele está pronto para uma PR?
   ```

3. O agente de QA começará a trabalhar.
4. Leia o relatório apresentado quando o trabalho terminar.

## Resumo e próximas etapas

Você adicionou uma função especializada reutilizável ao fluxo de trabalho e revisou o trabalho do agente. Nesta lição, você:

- explorou como um agente personalizado trabalha com instruções, skills e ferramentas MCP.
- criou e inspecionou um perfil reutilizável de garantia de qualidade.
- selecionou o agente de QA e revisou as descobertas dele em relação à issue de filtragem.

Agora você tem a implementação, a atualização da skill, o perfil de QA, os testes e o relatório de verificação prontos para revisão. Na próxima lição, você [reunirá tudo em uma PR do recurso e usará o Agent Merge][next-lesson].

## Recursos

- [Crie agentes personalizados para o Copilot CLI][create-agents]
- [Configuração de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
