---
title: "Exercício 2 - Agente na nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

É provável que poucas organizações não enfrentem problemas com dívida técnica. Ela pode incluir problemas de segurança não resolvidos, código legado que precisa de atualizações ou solicitações de funcionalidades que ficaram paradas no backlog por falta de tempo para implementá-las. O agente na nuvem do GitHub Copilot foi criado para tarefas como atualizar código e adicionar funcionalidades de forma autônoma. Quando termina o trabalho, ele gera uma PR em rascunho para que um desenvolvedor revise. Assim, você pode delegar tarefas tediosas, acelerar o desenvolvimento e se concentrar em questões mais abrangentes.

Neste exercício, você explorará o agente na nuvem do Copilot:

- personalizando o ambiente usado para gerar código.
- garantindo que as operações sejam executadas com segurança.
- entendendo a importância de issues com escopo claro.
- atribuindo issues ao Copilot.

## Cenários

A Tailspin Toys tem algumas dívidas técnicas que gostaria de resolver. Os prestadores de serviço contratados inicialmente para criar a primeira versão do site deixaram a documentação em um estado inadequado — na verdade, você verá que ela está totalmente ausente. Como primeira etapa, a equipe quer adicionar comentários de documentação TSDoc a todas as funções exportadas do aplicativo.

Além disso, a equipe de design quer melhorar a descoberta de jogos. Ela gostaria que a página de detalhes de cada jogo mostrasse outros jogos relacionados — títulos da mesma categoria — para que as pessoas continuem navegando. Ainda não é necessária uma implementação refinada; a equipe quer apenas algo que possa usar para testar e aceitar a experiência do usuário. Isso está bloqueando o trabalho, mas há outras issues com prioridade maior no momento.

Esses são dois exemplos de tarefas que podem ser despriorizadas com facilidade e que são ideais para atribuir ao agente na nuvem do Copilot. Ele pode trabalhar nelas de forma assíncrona, permitindo que o desenvolvedor se concentre em outras tarefas e depois volte para revisar o trabalho e confirmar se tudo está correto.

## Apresentação do agente na nuvem do GitHub Copilot

O [agente na nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) pode executar tarefas em segundo plano de maneira semelhante a um desenvolvedor. Assim como no trabalho com um desenvolvedor, há várias maneiras de iniciar uma tarefa, incluindo [atribuir uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois de receber a atribuição, o Copilot cria uma pull request em rascunho para acompanhar o progresso, configura um ambiente e começa a trabalhar. Você pode examinar a sessão do Copilot enquanto o trabalho está em andamento ou depois que ele termina. Quando a solução proposta estiver pronta para revisão, o Copilot mencionará você na pull request.

## A importância de instruções com escopo claro

Não há mágica no GitHub Copilot: você não pode pular a etapa de pensar. Até operações aparentemente simples podem se tornar complexas quando examinadas de perto; por isso, [defina cuidadosamente o escopo das tarefas para o agente na nuvem do Copilot][cloud-agent-best-practices]. Trate-o como um parceiro de programação com IA: trabalhe em etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software não mudam com a inclusão da IA generativa.

## Instruções personalizadas neste repositório

Nos exercícios anteriores, você conheceu as instruções personalizadas e como elas orientam o Copilot. Se você já concluiu um exercício prático sobre instruções personalizadas, viu o arquivo `.github/copilot-instructions.md` em ação. Se não, este é um bom momento para examiná-lo. Antes de atribuir trabalho ao agente na nuvem do Copilot, leia os arquivos de instruções já incluídos neste repositório para perceber a influência deles mais adiante.

Abra os arquivos a seguir na interface Web do GitHub ou em um codespace, se já tiver um em execução:

- `.github/copilot-instructions.md` — revise a seção **Code standards**, principalmente as expectativas para convenções TypeScript e comentários de documentação TSDoc.
- `.github/instructions/unit-tests.instructions.md` — observe o frontmatter `applyTo`, que aplica essas instruções aos arquivos `**/*.test.ts`.

Quando você atribuir ao agente na nuvem a issue `Code lacks documentation` na próxima seção, observe na pull request resultante os comentários de documentação TSDoc, as convenções TypeScript e os cabeçalhos de comentário. Essas orientações vêm dos arquivos de instruções. Voltaremos a isso ao revisar as PRs em um exercício posterior.

## Configure o ambiente de desenvolvimento do agente na nuvem do Copilot

Normalmente, a criação de código exige um ambiente específico e a execução de scripts de configuração para garantir que tudo esteja pronto. O mesmo vale ao atribuir tarefas ao Copilot, que trabalha de forma semelhante a um engenheiro de software.

O agente na nuvem usa o [GitHub Actions][github-actions] como ambiente de trabalho. Você pode personalizar esse ambiente criando um [workflow de configuração especial][setup-workflow] no arquivo `.github/workflows/copilot-setup-steps.yml`, que será executado antes de o agente começar. Assim, ele terá acesso às ferramentas de desenvolvimento e dependências necessárias. O workflow já está configurado neste laboratório para facilitar o aprendizado. Ele dá ao Copilot acesso ao Node.js, às dependências do projeto, aos binários do navegador usados nos testes de ponta a ponta e ao banco de dados SQLite migrado e preenchido com dados para o aplicativo Astro único:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões no nível mínimo necessário para as etapas. O Copilot recebe um token próprio para as operações dele.
    permissions:
      # Para clonar o repositório nas etapas de configuração, por exemplo, para instalar dependências, será necessária a permissão `contents: read`. Se você não clonar o repositório nas etapas de configuração, o Copilot fará isso automaticamente depois que elas forem concluídas.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do app: Node.js (o app inteiro agora é Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra e preenche o banco de dados SQLite local para que builds e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

Esse arquivo se parece com qualquer workflow do GitHub, mas tem alguns pontos importantes:

- Ele contém um único job chamado `copilot-setup-steps`. Esse job é executado no GitHub Actions antes de o Copilot começar a trabalhar na pull request.
- Observe o gatilho `workflow_dispatch`, que permite executar o workflow manualmente pela guia **Actions** do repositório. Isso é útil para testar o workflow em vez de aguardar o Copilot executá-lo.

## Adicione documentação

Todos reconhecem a importância da documentação, mas muitos projetos têm informações desatualizadas ou não têm documentação. Essa dívida técnica pode reduzir a produtividade e dificultar a manutenção da base de código ou a integração de novos desenvolvedores. Felizmente, o Copilot é muito útil para criar documentação, então esta é uma issue ideal para atribuir ao agente na nuvem do Copilot. Ele trabalhará em segundo plano para gerar a documentação necessária. Em um exercício futuro, você voltará para revisar o resultado.

1. Em uma nova guia do navegador, acesse seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para criar uma issue em branco.
5. Defina o **Title** como `Code lacks documentation`.
6. Defina a **Description** como:

   ```plaintext
   Nossa organização exige que funções e métodos incluam comentários de documentação TSDoc quando forem úteis. Infelizmente, as atualizações recentes não seguiram esse padrão. Precisamos atualizar o código existente para incluir comentários de documentação onde eles esclareçam o comportamento.
   ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

   ![Atribuição do Copilot a uma issue](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.
10. Selecione a guia **Pull Requests**.
11. Abra a pull request (PR) criada pelo Copilot, com um título semelhante a `[WIP]: Code lacks documentation`. Se a nova PR ainda não aparecer na lista, aguarde um ou dois minutos e atualize a janela do navegador.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas pode levar alguns minutos para aparecer na PR. O Copilot está criando o ambiente (executando o workflow descrito anteriormente), analisando o projeto e determinando a melhor abordagem para o problema.

13. Revise a lista e as tarefas que o Copilot pretende concluir.
14. Role a linha do tempo da pull request. Você deverá ver uma atualização indicando que o Copilot começou a trabalhar na issue.
15. Selecione o botão **View session**.

   ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Observe que você pode percorrer a sessão em tempo real e acompanhar como o Copilot resolve o problema: ele explora o código e entende o estado atual, faz pausas para analisar e escolher um plano adequado e também cria código.

Isso provavelmente levará alguns minutos. Um dos principais objetivos do agente na nuvem do Copilot é executar tarefas de forma assíncrona, permitindo que você se concentre em outras atividades. Vamos aproveitar esse recurso atribuindo outra tarefa ao agente na nuvem e, em seguida, voltando nossa atenção à criação de funcionalidades para o aplicativo.

## Adicione uma seção de jogos relacionados à página de detalhes

Como já vimos, uma das grandes vantagens do agente na nuvem do GitHub Copilot é poder dividir o trabalho: enquanto você se concentra em um conjunto de tarefas, ele cuida de outro. Talvez a seção de jogos relacionados para a equipe de design não leve muito tempo, mas esse tempo poderia ser usado em outras atividades. Vamos atribuí-la ao agente na nuvem do Copilot.

1. Volte ao repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para usar o template em branco.
5. Defina o **Title** como `Show related games on the game details page`.
6. Defina a **Description** como:

   ```markdown
   Queremos ajudar as pessoas a descobrir mais jogos mostrando jogos relacionados na página de detalhes de cada jogo. A equipe de design quer explorar a experiência do usuário e fazer testes de aceitação. Os requisitos são:

   - Adicionar um helper de acesso a dados em `src/lib/` que retorne outros jogos da mesma categoria de um jogo, sem incluir o próprio jogo
   - Mostrar uma seção "Related games" na página de detalhes do jogo usando o helper
   - Tratar o caso em que um jogo não tem jogos relacionados
   - Criar testes unitários para o novo helper
   - Antes de criar a PR, confirmar se todos os testes passam
   ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

   ![Atribuição do Copilot a uma issue](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Pouco depois, você verá um emoji 👀 no primeiro comentário da issue, indicando que o Copilot começou a trabalhar.

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

Agora o agente na nuvem do Copilot está trabalhando diligentemente na segunda solicitação. Ele funciona de maneira semelhante a um engenheiro de software, então você não precisa acompanhá-lo ativamente: basta revisar o trabalho quando ele terminar. Vamos voltar nossa atenção para a criação e o uso de agentes personalizados.

## Resumo e próximas etapas

Este exercício apresentou o [agente na nuvem do GitHub Copilot][copilot-agents]. Com o agente na nuvem, você pode atribuir issues ao Copilot para execução assíncrona. Ele pode ajudar a resolver dívidas técnicas, criar funcionalidades ou migrar código de uma estrutura para outra.

Neste exercício, você explorou:

- como personalizar o ambiente usado para gerar código.
- como garantir que as operações sejam executadas com segurança.
- a importância de issues com escopo claro.
- como atribuir issues ao Copilot.

Enquanto o agente na nuvem trabalha em segundo plano, vamos criar e usar agentes personalizados. O [agente na nuvem do Copilot também pode usar servidores MCP][cloud-agent-mcp] e tem acesso a instruções personalizadas, que já exploramos em módulos anteriores.

## Recursos

- [Sobre o agente na nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente na nuvem][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
