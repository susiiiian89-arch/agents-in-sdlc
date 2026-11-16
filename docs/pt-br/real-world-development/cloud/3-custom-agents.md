---
title: "Exercício 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

[Agentes personalizados][custom-agents-concept] no GitHub Copilot permitem criar assistentes de IA especializados para tarefas ou domínios específicos do fluxo de desenvolvimento. Ao definir agentes em arquivos Markdown na pasta `.github/agents` do repositório, você fornece ao Copilot instruções focadas, práticas recomendadas, padrões de código e conhecimento do domínio para ajudá-lo a executar determinados tipos de trabalho com mais eficiência. As equipes podem registrar seu conhecimento em agentes reutilizáveis — por exemplo, um agente de acessibilidade que aplica a conformidade com [WCAG][wcag], um agente de segurança que segue práticas de codificação segura ou um agente de testes que mantém padrões de teste consistentes.

Os agentes personalizados são definidos em arquivos Markdown na pasta `.github/agents` do projeto ou globalmente em `~/.copilot/agents`. Cada arquivo tem frontmatter YAML com pelo menos os campos `name` e `description`, seguidos de um prompt Markdown que define o comportamento, a especialização e as instruções do agente.

### Agentes personalizados e skills de agente

Há alguma sobreposição entre agentes personalizados e [skills de agente][agent-skills-concept]. Ambos são definidos principalmente em arquivos Markdown e orientam uma IA sobre como executar operações. A distinção mais simples é: um **agente personalizado** é quem executa o trabalho, e as **skills** são as ferramentas.

Os agentes personalizados têm sua própria janela de contexto e podem orquestrar skills (e até outros agentes) para realizar o trabalho. Neste laboratório, o agente personalizado de acessibilidade revisará e atualizará o site de acordo com as diretrizes de acessibilidade. Como parte do trabalho, ele poderá chamar skills, como uma skill para o fluxo de pull requests ou outra que execute e gerencie testes.

> [!NOTE]
> Não há uma única maneira correta de criar um agente personalizado. Como em qualquer recurso de IA, teste e faça iterações para descobrir o que funciona nos seus ambientes e cenários.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Cenário

A Tailspin Toys quer garantir que sua plataforma de financiamento coletivo seja acessível a todos, independentemente das habilidades ou preferências visuais. Comentários recentes de usuários indicam que algumas pessoas têm dificuldade para ler o tema escuro atual por causa do contraste insuficiente entre as cores do texto e do plano de fundo. Para resolver essa questão de acessibilidade, a equipe de design solicitou um modo de alto contraste que os usuários possam ativar e desativar.

Como a acessibilidade é essencial, queremos implementar isso o mais rápido possível. Você usará um agente personalizado para gerar a funcionalidade.

Neste exercício, você vai:

- revisar um agente personalizado de acessibilidade existente.
- usar o agente de acessibilidade no Copilot Chat para implementar um modo de alto contraste.

## Revise o agente personalizado de acessibilidade

Um agente personalizado de acessibilidade já foi criado. Vamos revisar o arquivo para entender como ele orientará o Copilot.

Volte ao seu codespace e abra o arquivo do agente personalizado:

1. Abra `.github/agents/accessibility.md`.
2. Observe o frontmatter YAML com os campos `name` e `description`.

> [!CAUTION]
> Os campos `name` e `description` no frontmatter são obrigatórios para agentes personalizados.

3. Leia as seções seguintes, que destacam:
   - responsabilidades principais ao gerar código para um site acessível.
   - práticas recomendadas de acessibilidade.
   - exemplos de código para HTML, CSS e JavaScript.
   - armadilhas e erros comuns.

## Crie e atribua uma issue

O painel de agentes é o local central para trabalhar com todos os agentes no seu ambiente. Você pode atribuir tarefas ao agente na nuvem do Copilot, monitorar tarefas e até redirecionar uma sessão em andamento e fornecer orientações adicionais. Vamos atribuir ao Copilot a tarefa de criar o modo de alto contraste.

1. Acesse seu repositório.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para criar uma nova issue.
5. Defina o **Title** como `Add high contrast mode to website`.
6. Defina a **Description** como:

   ```plaintext
   Precisamos de um modo de alto contraste para o site. Deve haver um controle que o usuário possa ativar para usar o alto contraste. A configuração deve ser armazenada no armazenamento local do navegador.
   ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.
9. Selecione **Accessibility agent** na lista de agentes personalizados.

   ![Caixa de diálogo de atribuição do agente na nuvem, com o agente personalizado de acessibilidade destacado](../../../_images/ex5-select-custom-agent.png)

10. Selecione **Assign**.
11. O Copilot começará a trabalhar na tarefa em segundo plano.

## Resumo e próximas etapas

Este exercício apresentou os [agentes personalizados][custom-agents] do GitHub Copilot, assistentes de IA especializados em tarefas e domínios específicos. Com agentes personalizados, você pode registrar o conhecimento e os padrões da equipe em agentes reutilizáveis que orientam o Copilot na execução de tipos específicos de trabalho.

Neste exercício, você explorou:

- como definir agentes personalizados.
- como atribuir uma tarefa a um agente personalizado.

Enquanto o Copilot trabalha na implementação do modo de alto contraste, vamos [acompanhar e orientar a sessão do agente][next-lesson] pelo painel de agentes.

## Recursos

- [Sobre os agentes personalizados][custom-agents]
- [Preparar sua organização para usar agentes personalizados][org-custom-agents]
- [Preparar sua empresa para usar agentes personalizados][enterprise-custom-agents]

---

| [← Lição anterior: Agente na nuvem do GitHub Copilot][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
