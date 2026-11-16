---
title: "Exercício 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot permitem criar assistentes de IA especializados em tarefas ou domínios específicos do seu fluxo de desenvolvimento. Ao definir agentes em arquivos Markdown na pasta `.github/agents` do repositório, você fornece ao Copilot instruções focadas, práticas recomendadas, padrões de codificação e conhecimentos específicos do domínio para ajudá-lo a executar determinados tipos de trabalho com mais eficiência. As equipes podem codificar seus conhecimentos em agentes reutilizáveis — por exemplo, um agente de acessibilidade que aplica a conformidade com [WCAG][wcag], um agente de segurança que segue práticas de codificação segura ou um agente de testes que mantém padrões de teste consistentes.

Os agentes personalizados são definidos em arquivos Markdown na pasta `.github/agents` do projeto ou globalmente em `~/.copilot/agents`. Cada arquivo tem um frontmatter YAML com pelo menos `name` e `description`, seguido de um prompt Markdown que define o comportamento, a especialidade e as instruções do agente.

### Comparação entre agentes personalizados e skills de agente

Agentes personalizados e [skills de agente][agent-skills-concept] têm algumas semelhanças. Ambos são definidos principalmente em arquivos Markdown e orientam uma IA sobre como executar tarefas. A maneira mais simples de diferenciá-los é: um **agente personalizado** é o executor; as **skills** são as ferramentas.

Os agentes personalizados têm sua própria janela de contexto e foram criados para orquestrar skills (e até outros agentes) durante o trabalho. Neste laboratório, o agente personalizado de acessibilidade revisará e atualizará o site de acordo com as diretrizes de acessibilidade. Durante o trabalho, ele poderá chamar skills, como uma skill para o fluxo de pull requests ou outra para executar e gerenciar testes.

> [!NOTE]
> Não há uma única maneira certa de criar um agente personalizado. Como em tudo que envolve IA, teste e faça iterações para descobrir o que funciona em seus ambientes e cenários.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Cenário

A Tailspin Toys está comprometida em tornar sua plataforma de financiamento coletivo acessível a todos, independentemente da capacidade visual ou das preferências de cada pessoa. Comentários recentes de usuários indicaram que algumas pessoas têm dificuldade para ler o tema escuro atual devido ao contraste insuficiente entre as cores do texto e do plano de fundo. Para resolver esse problema de acessibilidade, a equipe de design solicitou a implementação de um modo de alto contraste que os usuários possam ativar e desativar.

Como a acessibilidade é essencial, você quer implementar isso o mais rápido possível. Para gerar a funcionalidade, você usará um agente personalizado.

Neste exercício, você vai:

- revisar um agente personalizado de acessibilidade existente.
- usar o agente de acessibilidade no Copilot Chat para implementar um modo de alto contraste.

## Revise o agente personalizado de acessibilidade

Um agente personalizado de acessibilidade já foi criado para você. Vamos revisar o conteúdo para entender como ele orientará o Copilot.

Volte ao seu codespace, abra um terminal e mude para uma nova branch criada a partir de `main` para trabalhar na acessibilidade (o trabalho da PR de filtragem do Exercício 3 ficará separado):

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abra `.github/agents/accessibility.md`.
2. Observe o frontmatter YAML com os campos `name` e `description`.

> [!CAUTION]
> Os campos `name` e `description` no frontmatter são obrigatórios para agentes personalizados.

3. Em seguida, leia as seções que descrevem:
   - as principais responsabilidades ao gerar código para um site acessível.
   - as práticas recomendadas de acessibilidade.
   - exemplos de código HTML, CSS e JavaScript.
   - problemas e erros comuns.

## Use o agente personalizado no Copilot Chat

O VS Code exibe todos os agentes personalizados definidos em `.github/agents` no menu suspenso de agentes, na parte inferior do painel Copilot Chat. Você pode selecionar um agente personalizado para aplicar as instruções e ferramentas dele à sessão de chat.

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao seu codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e a seleção do modelo variam por exercício — cada etapa indicará o que é relevante.

1. Se **Agent** ainda não estiver selecionado, escolha-o no menu suspenso de agentes da exibição Chat.

   ![Captura de tela do seletor de agentes na exibição Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecione o menu suspenso de agentes na parte inferior da exibição Chat. Ele mostra o agente ativo — por padrão, **default**.
3. Selecione **Accessibility agent** na lista de agentes disponíveis.
4. Envie o seguinte prompt ao agente de acessibilidade:

    ```plaintext
    Adicione um modo de alto contraste ao site. Deve haver um controle que o usuário possa ativar para usar o alto contraste, e a configuração deve persistir após recarregar a página usando o armazenamento local do navegador.
    ```

5. O Copilot Chat começará a trabalhar — explorará a base de código, proporá alterações e editará os arquivos do projeto. Cada edição aparecerá na conversa com o caminho do arquivo e um diff para revisão.
6. À medida que as edições forem aplicadas, o indicador **Files changed** será atualizado com os arquivos alterados pelo agente.

> [!NOTE]
> Esse processo provavelmente levará alguns minutos. O Copilot está fazendo alterações reais no repositório — editará arquivos existentes, como componentes Astro, CSS e testes relacionados.

Você revisará e orientará o trabalho em andamento no próximo exercício.

## Resumo e próximas etapas

Nesta lição, você explorou os [agentes personalizados][custom-agents], assistentes especializados de IA do GitHub Copilot para tarefas e domínios específicos. Com agentes personalizados, você pode codificar o conhecimento e os padrões da equipe em agentes reutilizáveis que orientam o Copilot a executar determinados tipos de trabalho com mais eficiência.

Você explorou estes conceitos:

- como os agentes personalizados são definidos.
- como usar um agente personalizado no modo Agent do Copilot Chat.

Na próxima lição, você [acompanhará e orientará o trabalho do agente][next-lesson] — revisando as alterações à medida que são feitas e adicionando um alternador para o modo claro à mesma sessão.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Crie agentes personalizados no seu IDE][creating-custom-agents-ide]
- [Agentes personalizados no VS Code][custom-agents-vscode]
- [Configuração de agentes personalizados][custom-agents-config]
- [Agentes personalizados no awesome-copilot][awesome-copilot-agents]

---

| [← Lição anterior: Teste seu recurso com o servidor MCP do Playwright][previous-lesson] | [Próxima lição: Monitore e gerencie agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
