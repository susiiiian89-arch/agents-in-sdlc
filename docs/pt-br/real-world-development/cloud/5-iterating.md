---
title: "Exercício 5: Iterar sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revise o trabalho

Ao longo deste laboratório, você trabalhou com o GitHub Copilot em várias tarefas para melhorar a experiência do usuário e adicionar funcionalidades. Você pediu ao Copilot que documentasse o código, criasse um recurso de jogos relacionados para a equipe de design testar e implementasse recursos de acessibilidade, incluindo controles para os modos de alto contraste e claro. Vamos revisar as alterações no código e, se necessário, enviar comentários ao Copilot para melhorar o resultado.

### Cenário

Como já destacamos várias vezes, os fundamentos de design de software e DevOps não mudam com a inclusão da IA generativa. Sempre precisamos revisar o código gerado e seguir nosso processo normal de DevOps. Com isso em mente, vamos revisar as sugestões do GitHub Copilot para criar documentação, implementar o recurso de jogos relacionados e adicionar recursos de acessibilidade antes de habilitar a revisão para o restante da equipe.

## Segurança e agente na nuvem do GitHub Copilot

Como o agente na nuvem do Copilot executa tarefas de forma assíncrona e sem supervisão, existem algumas restrições de segurança para manter tudo protegido:

- O Copilot tem acesso somente de leitura ao repositório e acesso de gravação **apenas** à branch que usará para o código.
- O agente na nuvem é executado no GitHub Actions, que cria um ambiente separado e temporário para o trabalho.
- Um humano precisa aprovar todos os workflows do GitHub Actions antes da execução.
- [O acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluindo servidores MCP.

## Revise a documentação gerada

Vamos começar examinando a primeira pull request (PR) criada pelo agente na nuvem do GitHub Copilot: a que adiciona documentação ao código. Você fará isso na interface padrão de PR do GitHub.com.

> [!NOTE]
> Ao abrir a PR, talvez você veja um aviso de que o GitHub Copilot está bloqueado por um firewall. Isso é **esperado**, pois o Copilot tem acesso limitado a recursos externos por padrão, incluindo chamadas a servidores MCP externos. Se quiser, você pode [personalizar ou desabilitar o firewall do agente na nuvem do Copilot][agent-firewall].

1. Volte ao repositório no github.com.
2. Selecione **Pull Requests** para abrir a lista de pull requests.
3. Abra a pull request intitulada **Add missing documentation** ou com um título mais elaborado.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, a pull request terá a indicação **[WIP]**. Nesse caso, aguarde a conclusão do trabalho. Isso pode levar alguns minutos, então você pode fazer uma pausa ou refletir sobre o que aprendeu até agora.

4. Quando a pull request estiver pronta, selecione a guia **Files changed** e revise as alterações.

   ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Examine o código atualizado, que inclui os comentários de documentação TSDoc e outros documentos criados. As alterações exatas variam.

   Ao examinar as alterações, procure comentários TSDoc, convenções TypeScript e cabeçalhos de comentário. Eles seguem os arquivos de instruções personalizadas que você revisou no início do Exercício 2.

6. Depois de revisar as atualizações e confirmar que estão corretas, volte à guia **Conversation** e role a página para baixo.
7. Você deverá ver um indicador de que alguns workflows estão aguardando aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Botão Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Os workflows deverão aparecer na fila da seção de verificações da pull request. Se tudo correr bem, as verificações do projeto para o aplicativo Astro único passarão. Isso pode levar alguns minutos.

## Peça alterações ao GitHub Copilot

Trabalhar com o Copilot em uma pull request não é uma via de mão única. Você também pode mencionar o Copilot em comentários da PR — como faria com outra pessoa da equipe — ou em comentários de linha do código. O Copilot verá esses comentários e iniciará outra sessão para atendê-los. Como os resultados são não determinísticos, não podemos fornecer um texto exato para esses pedidos. Algumas ideias de alterações que você pode solicitar:

- Adicionar cabeçalhos de comentário no início de cada arquivo de código, com uma descrição breve do que fazem.
- Adicionar comentários de documentação TSDoc aos arquivos TypeScript e Astro.
- Criar um README que descreva a estrutura do aplicativo Astro.

1. Adicione um comentário solicitando uma alteração na documentação gerada e mencione **@copilot**, como faria com qualquer pessoa. Use uma das ideias acima ou sugira outra alteração de documentação que gostaria de ver na base de código.
2. Selecione **View Session** para acompanhar o trabalho do Copilot. Observe como ele inicia uma nova sessão para fazer as atualizações.
3. Selecione **Back to pull request** para voltar à pull request.

   ![Botão Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Quando o Copilot concluir as alterações, você verá um novo commit na pull request.
5. Selecione a guia **Files changed** e revise as alterações.

Continue fazendo iterações até ficar satisfeito. Depois, você poderá marcar a PR como pronta para revisão e fazer o merge na branch principal.

![Opção para marcar a PR como pronta para revisão](../../../_images/ex7-ready-for-review.png)

## Revise o recurso de jogos relacionados

Vamos voltar à PR criada pelo Copilot para resolver a issue sobre a exibição de jogos relacionados na página de detalhes de cada jogo.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione a PR com um título semelhante a **Show related games on the game details page** ou algo mais elaborado.
4. Selecione a guia **Files changed** para revisar o código gerado.
5. Depois de revisar as atualizações e confirmar que estão corretas, volte à guia **Conversation** e role a página para baixo.
6. Você deverá ver um indicador de que alguns workflows estão aguardando aprovação.
7. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Botão Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. Os workflows deverão aparecer na fila da seção de verificações da pull request. Se tudo correr bem, as verificações do projeto para o aplicativo Astro único passarão. Isso pode levar alguns minutos.
9. **Opcional:** você pode mudar para essa branch no Codespace para testar manualmente o recurso de jogos relacionados. Acesse o Codespace, abra o terminal e execute os comandos a seguir (substitua `<branch-name>` pelo nome da branch criada pelo Copilot, por exemplo, **copilot/fix-8**):

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

O Copilot criou o recurso de jogos relacionados. Como antes, você pode pedir alterações ao agente na nuvem e trabalhar iterativamente. Por exemplo, talvez queira alterar a quantidade de jogos relacionados exibidos ou solicitar cabeçalhos de comentário e comentários TSDoc (lembre-se: a tarefa foi atribuída **antes** da atualização das instruções personalizadas). Para fazer esses pedidos, adicione um comentário à guia **Conversation**; o Copilot o verá e iniciará outra sessão.

## Revise os recursos de acessibilidade

Por fim, vamos revisar os recursos de acessibilidade implementados com o agente personalizado de acessibilidade. Esta PR deve incluir o modo de alto contraste atribuído no Exercício 3 e o modo claro solicitado no painel central do Exercício 4.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione a PR com um título semelhante a **Add high contrast mode to website** ou algo mais elaborado.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, a pull request terá a indicação **[WIP]**. Nesse caso, aguarde a conclusão do trabalho. Isso pode levar alguns minutos.

4. Selecione a guia **Files changed** para revisar o código gerado.
5. Revise a implementação e preste atenção especial a:
   - componentes de interface para alternar entre os modos.
   - uso do armazenamento local para persistir as preferências.
   - alterações de CSS ou estilo dos modos de alto contraste e claro.
   - atributos de acessibilidade (rótulos ARIA, navegação pelo teclado etc.).
   - código JavaScript/TypeScript que controla a alternância entre os modos.
6. Depois de revisar as atualizações e confirmar que estão corretas, volte à guia **Conversation** e role a página para baixo.
7. Você deverá ver um indicador de que alguns workflows estão aguardando aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Botão Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Os workflows deverão aparecer na fila da seção de verificações da pull request. Se tudo correr bem, as verificações do projeto para o aplicativo Astro único passarão. Isso pode levar alguns minutos.
10. **Opcional:** você pode mudar para essa branch no Codespace para testar manualmente os recursos de acessibilidade. Acesse o Codespace, abra o terminal e execute estes comandos (substitua `<branch-name>` pelo nome da branch criada pelo Copilot):

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   Em seguida, inicie o aplicativo e teste os controles dos modos de alto contraste e claro no navegador. Confirme se funcionam como esperado e se as preferências persistem após recarregar a página.

Observe como o agente personalizado de acessibilidade ajudou a orientar o Copilot na implementação desses recursos de acordo com as práticas recomendadas de acessibilidade. Se encontrar problemas de acessibilidade ou oportunidades de melhoria, mencione **@copilot** em um comentário para solicitar alterações, como fez com as PRs anteriores.

## Exercício opcional — continue delegando

O agente na nuvem funciona melhor quando você pode atribuir issues reais do backlog e se concentrar em outras atividades. Para desenvolver esse hábito, crie mais algumas issues no repositório e atribua-as ao Copilot. Algumas ideias:

- Criar um formulário para manifestar interesse em apoiar o jogo na página de detalhes.
- Implementar paginação na lista de jogos.
- Adicionar validação de entrada e tratamento de erros aos helpers de acesso a dados.

## Resumo

Você concluiu o roteiro do agente na nuvem. Nestas lições, você:

- **Examinou os arquivos de instruções personalizadas incluídos no repositório** para ver como influenciam o trabalho do agente na nuvem.
- **Atribuiu issues ao agente na nuvem do Copilot** e acompanhou a configuração do ambiente, o planejamento e a execução assíncrona.
- **Criou e usou um agente personalizado** para acessibilidade, adicionando controles para os modos de alto contraste e claro.
- **Usou a página de agentes do Copilot como painel central** para acompanhar e orientar a sessão de acessibilidade.
- **Revisou as pull requests do agente na nuvem e fez iterações**, mencionando `@copilot` para solicitar alterações e aprovando workflows.

## Revisão e próximas etapas

Você concluiu o roteiro do agente na nuvem. Para continuar explorando, os outros roteiros complementam o que praticou:

- 🖥️ **[Roteiro do VS Code](../../vscode/)** — explore o modo Agent do Copilot Chat e a integração com MCP diretamente no IDE.
- 💻 **[Roteiro do CLI](../../cli/)** — entregue duas alterações revisadas no terminal usando os modos Plan e Autopilot, skills de agente, um agente de QA personalizado, Playwright MCP, Agent Merge e uma ponte opcional `/delegate` para voltar ao agente na nuvem.

No seu próprio repositório, experimente estas ideias:

- Atribua ao agente na nuvem uma issue de refatoração ou de cobertura de testes.
- Crie um agente personalizado para outro domínio.
- Configure `copilot-setup-steps.yml` para outra pilha tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre os agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente na nuvem][cloud-agent-best-practices]
- [Configurar o firewall do agente na nuvem do Copilot][agent-firewall]

---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
