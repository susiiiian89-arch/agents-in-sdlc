---
title: "Exercício 2 - Adicione novas funcionalidades com o Agent Mode do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até as atualizações mais simples de um app normalmente exigem alterações em vários arquivos e a execução de operações como testes. O fluxo de trabalho típico de um desenvolvedor envolve localizar todos os arquivos necessários, fazer as alterações, executar os testes, depurar, descobrir qual arquivo ficou faltando, fazer outra atualização... E por aí vai.

É aí que entra o Agent Mode do Copilot.

O Agent Mode do Copilot foi desenvolvido para agir com mais autonomia no IDE. Ele funciona de maneira semelhante a um desenvolvedor: começa explorando a estrutura do projeto, faz as atualizações necessárias, executa tarefas como testes e corrige automaticamente os problemas encontrados. Vamos explorar como usar o Agent Mode para adicionar novas funcionalidades ao site.

> [!NOTE]
> Apesar de terem nomes semelhantes, o modo Agent e o agente na nuvem oferecem experiências diferentes. O modo Agent executa tarefas no IDE, permitindo ciclos rápidos de feedback e interação. O agente na nuvem funciona como um programador parceiro assíncrono, como alguém da equipe, e interage com você por meio de issues e pull requests.

Neste exercício, você aprenderá como:

- o Agent Mode do Copilot pode explorar seu projeto, identificar arquivos relevantes e coordenar alterações.
- o Agent Mode do GitHub Copilot pode implementar novos recursos na interface e na camada de dados.
- revisar alterações e testes gerados pelo Agent Mode do Copilot antes de fazer merge na base de código.

## Cenário

À medida que a lista de jogos cresce, você quer permitir que os usuários filtrem por categoria e editora. No exercício anterior, você adicionou um helper de editoras. Agora, concluirá o trabalho restante na camada de dados, na interface e nos testes com o Agent Mode do Copilot.

## Execute o site Tailspin Toys

Antes de fazer alterações, vamos explorar o site Tailspin Toys para entender a funcionalidade atual.

O site é uma plataforma de financiamento coletivo para jogos de tabuleiro com um tema voltado a desenvolvedores. Ele permite listar jogos e exibir detalhes sobre eles. O site é um único app Astro que renderiza suas páginas como HTML estático no momento da compilação. As páginas consultam diretamente um banco de dados SQLite local no frontmatter, usando o Drizzle ORM — não há uma API de back-end separada nem uma estrutura de interface no lado do cliente. Os helpers reutilizáveis de acesso a dados ficam em `src/lib/`, e a interatividade é adicionada com um pequeno `<script>` Astro com escopo local e APIs padrão do DOM.

### Inicie o site

Para facilitar a execução do site, há uma tarefa `npm run dev` que inicia o único servidor de desenvolvimento Astro. Siga estas etapas para executá-la no GitHub Codespace:

1. Volte ao seu codespace. Você deve estar na branch `main` depois do exercício anterior.
2. Abra uma nova janela de terminal no Codespace pressionando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crie uma nova branch e mude para ela para trabalhar na filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Execute o seguinte comando para iniciar o site:

   ```bash
   npm run dev
   ```

   Quando o servidor Astro estiver pronto, você verá um banner com o URL, semelhante ao exemplo abaixo:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se uma caixa de diálogo solicitar que você abra uma janela do navegador para `http://localhost:4321`, feche-a selecionando o **x**.

5. Abra o site mantendo pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando o endereço do servidor Astro `http://localhost:4321` no terminal.

> [!NOTE]
> Ao usar um codespace, selecionar um link para o URL localhost no terminal do Codespace redirecionará automaticamente para `https://<your-codespace-name>-4321.app.github.dev/`. Esse é um túnel privado para seu codespace, que agora hospeda o servidor Web!

### Explore o site

Com o site em execução, você pode explorar os recursos disponíveis. Os principais recursos incluem:

- **Página inicial**: exibe uma lista de jogos de tabuleiro com títulos, imagens e descrições.
- **Página de detalhes do jogo**: ao selecionar um jogo, você verá uma página de detalhes com mais informações, incluindo título, descrição, editora e categoria.

## Explore o backlog com o Copilot

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao seu codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e a seleção do modelo variam por exercício — cada etapa indicará o que é relevante.

A implementação inicial do site funciona, mas queremos aprimorá-la com novos recursos. Vamos começar revisando o backlog. Ao configurar o projeto a partir do template, uma lista de issues foi criada automaticamente — peça ao GitHub Copilot para exibi-las.

1. Selecione **Agent** no menu suspenso de agentes da exibição Chat. O agente **Agent** planeja e implementa alterações em vários arquivos de forma autônoma, executa comandos de terminal e invoca ferramentas.

   ![Captura de tela do seletor de agentes na exibição Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecione **Claude Sonnet 4.5** na lista de modelos disponíveis.

> [!CAUTION]
> Os autores deste workshop não estão recomendando um modelo específico. Usamos o Claude Sonnet 4.5 ao criar o workshop e, por isso, o incluímos nas instruções. A intenção é que as sugestões de código sejam relativamente consistentes e proporcionem uma boa experiência. No entanto, como os LLMs são probabilísticos, é normal que as sugestões recebidas sejam diferentes das indicadas no workshop.

> [!NOTE]
> Devido à natureza probabilística dos LLMs, o Copilot pode usar um comando MCP diferente, mas ainda deverá conseguir concluir a tarefa.

3. Peça ao Copilot informações sobre as issues do backlog enviando o seguinte prompt:

   ```plaintext
   Mostre o backlog de itens do meu repositório GitHub e me ajude a priorizá-los com base no que será mais útil para os usuários.
   ```

4. Selecione **Continue** para executar o comando que lista todas as issues.
5. Revise a lista de issues gerada.

Observe como o Copilot também priorizou os itens de acordo com o que considera mais útil para o usuário.

## Revise os arquivos de instruções

Antes de pedir ao agente para gerar código, vale a pena revisar o arquivo de instruções que fornecerá contexto ao Copilot. Você usará o arquivo de [interface do usuário (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica como abordar a adição de funcionalidades ao site.

1. No seu codespace, acesse `.github/instructions/ui.instructions.md`.
2. Observe as orientações gerais para adicionar funcionalidades. Elas incluem:
   - uma visão geral da arquitetura.
   - princípios para design de componentes, testabilidade e acessibilidade.
   - links para arquivos de instruções específicos de vários tipos de arquivo, incluindo:
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> Os arquivos de instruções permitem fazer referência a outros arquivos de instruções e a arquivos do projeto. Os caminhos são relativos ao local do arquivo de instruções. Isso facilita a reutilização, permite dividir conjuntos complexos de instruções em partes menores e mais gerenciáveis e fornece exemplos e templates para o Copilot seguir.

## Implemente a funcionalidade de filtragem

Para concluir a filtragem, serão necessárias pelo menos três alterações separadas no app:

- adicionar ou aprimorar a lógica de filtragem restante na camada de dados (`src/lib/`).
- adicionar ou atualizar testes do comportamento de filtragem.
- atualizar a página de listagem de jogos para incluir a interface de filtragem.

Além disso, os testes precisam ser executados e aprovados antes do merge de tudo na base de código. O Agent Mode do Copilot pode executar essas tarefas! Vamos adicionar a funcionalidade.

1. Continue a conversa atual com o Copilot ou inicie uma nova selecionando **New Chat**.
2. Selecione **Add Context**, **Instructions** e **ui** como arquivo de instruções.

   ![Captura de tela de um exemplo de seleção do arquivo de instruções de UI](../../../_images/ex3-select-instructions-file.png)

3. Confirme se **Agent** continua selecionado no menu suspenso de agentes da exibição Chat.

   ![Captura de tela do seletor de agentes na exibição Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirme se **Claude Sonnet 4.5** continua selecionado como modelo.
5. Peça ao Copilot que implemente a funcionalidade de acordo com a issue relacionada no backlog, usando o seguinte prompt:

   ```plaintext
   Atualize o site para incluir filtros por editora e categoria de acordo com os requisitos da issue correspondente no backlog do GitHub. Um helper de editoras já existe por causa do exercício anterior; preserve e refine esse trabalho conforme necessário para concluir as alterações restantes na camada de dados, na interface e nos testes. Confirme se todos os testes passam antes de concluir. O servidor já está em execução, então não é necessário iniciá-lo.
   ```

6. Observe como o Copilot começa explorando o projeto e localizando os arquivos associados à funcionalidade desejada. Você verá o agente localizar os helpers da camada de dados, a interface e os testes. Em seguida, ele modificará os arquivos e executará os testes.

   ![Captura de tela do Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> O Copilot executará várias tarefas, como explorar o projeto, modificar arquivos e executar testes. Isso pode levar alguns minutos, dependendo da complexidade da tarefa e da base de código. Durante o processo, os botões **Keep** e **Undo** podem aparecer no editor. Quando o Copilot terminar, você verá **Keep** ou **Undo** para todas as alterações; não é necessário selecioná-los enquanto o trabalho estiver em andamento.

7. Quando solicitado pelo Copilot, selecione **Continue** para executar os testes.

   ![Captura de tela de uma caixa de diálogo no painel Copilot Chat pedindo confirmação para executar os testes](../../../_images/ex3-agent-mode-run-tests.png)

8. Talvez ocorram algumas pausas e até falhas em alguns testes durante o processo. Não tem problema! O Copilot alterna entre a geração de código e os testes até concluir a tarefa e não detectar erros.

   ![Captura de tela de uma sessão completa do Copilot Chat com o Agent Mode](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explore o código gerado para identificar possíveis problemas.

> [!CAUTION]
> Lembre-se: é importante revisar o código gerado pelo Copilot ou por qualquer outra ferramenta de IA.

10. Volte ao navegador em que o site está em execução e explore a nova funcionalidade!
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela Copilot Chat.

## Resumo e próximas etapas

Parabéns! Neste exercício, exploramos como usar o Agent Mode do GitHub Copilot para adicionar funcionalidades ao site Tailspin Toys. Aprendemos como:

- o Agent Mode do GitHub Copilot pode implementar novos recursos na interface e na camada de dados.
- o Agent Mode do Copilot pode explorar seu projeto, identificar arquivos relevantes e coordenar alterações.
- revisar alterações e testes gerados pelo Agent Mode do Copilot antes de fazer merge na base de código.

Agora vamos [testar seu recurso com o servidor MCP do Playwright][next-lesson] e abrir uma pull request.

### Exercício bônus de exploração – Implemente a paginação

À medida que a lista de jogos cresce, será necessário implementar paginação. Use as habilidades aprendidas neste exercício e peça ao Copilot que atualize o site para incluir paginação. Considere:

- seguir as práticas recomendadas existentes, incluindo o uso dos arquivos de instruções existentes.
- decidir como implementar a paginação: se o usuário poderá selecionar o tamanho da página ou se ele será fixo.
- fornecer ao Copilot as orientações necessárias para implementar a paginação da maneira desejada.
- talvez seja necessário iterar com o GitHub Copilot, pedir alterações e fornecer contexto. Esse é o fluxo normal ao trabalhar com o Copilot!

## Recursos

- [Modos ask, edit e agent do Copilot: o que fazem e quando usar cada um][choose-mode]
- [Modo Agent no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
