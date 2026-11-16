---
title: "Lição 12 - Publique o site com o GitHub Pages"
description: "Use o app GitHub Copilot para configurar um workflow do Pages, revisar a pull request e verificar o Space Quiz publicado."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Publique o Space Quiz no GitHub Pages para que qualquer pessoa possa acessá-lo. O Copilot pode preparar o workflow e orientar a publicação, mas você continua responsável por revisar as alterações e confirmar que o site está disponível.

Nesta lição, você vai:

- pedir ao Copilot que configure uma publicação no GitHub Pages.
- revisar o workflow e a pull request antes do merge.
- acompanhar a publicação e verificar o site publicado.

## Cenário

O Space Quiz já está em um repositório do GitHub, mas as pessoas ainda precisam de uma maneira simples de experimentá-lo. Publicar o site de arquivo único com o GitHub Pages disponibiliza uma URL pública sem adicionar um servidor ou uma dependência de runtime.

## Peça ao Copilot que prepare a publicação

> [!WARNING]
> Criar uma pull request pode confirmar e enviar código. Revise as configurações de aprovação e o repositório de destino antes de enviar este prompt. Com **Approve all**, não espere outra solicitação de confirmação.

Na sessão do app Copilot para `space-quiz`, envie este prompt:

```plaintext
Prepare uma publicação no GitHub Pages para este repositório. Crie um workflow do GitHub Actions que compile o site, se necessário, e o publique no GitHub Pages quando as alterações chegarem à branch padrão. Adicione um README que descreva o app e explique como executá-lo localmente, caso ainda não exista. Crie uma pull request com essas alterações e informe o link e quaisquer configurações ou permissões do Pages necessárias. Não faça o merge, não habilite o merge automático, não altere as configurações do Pages e não publique o site. Pare e aguarde minha revisão.
```

## Revise o workflow e a pull request

1. Confirme se o README foi adicionado somente se o repositório ainda não tinha um.
2. Revise as alterações da pull request, incluindo as etapas de compilação e publicação do workflow, os gatilhos e as permissões.
3. Confirme se o workflow publica a partir da branch padrão, e não de uma pull request ainda não mesclada.
4. Peça as correções necessárias e revise o diff atualizado. Aguarde a aprovação das verificações obrigatórias da pull request antes de autorizar o merge.

## Autorize o merge e a publicação

> [!WARNING]
> O prompt a seguir autoriza o agente a fazer o merge da pull request revisada, alterar as configurações do Pages do repositório e publicar o site. Envie-o somente depois de concluir a revisão e confirmar o repositório e a pull request. As configurações de aprovação podem permitir essas ações sem outra confirmação.

Substitua `<PR_NUMBER>` pelo número da pull request revisada e envie este prompt separado:

```plaintext
Revisei a pull request <PR_NUMBER> neste repositório e autorizo o merge e a publicação. Habilite o GitHub Pages usando o GitHub Actions, faça o merge dessa pull request somente depois que as verificações obrigatórias passarem e acompanhe a publicação até que seja concluída com sucesso. Informe a URL final do site publicado. Se você não tiver permissão para habilitar o Pages ou fazer o merge, diga exatamente o que preciso aprovar ou fazer; não informe sucesso até que o site esteja publicado.
```

![Diagrama em três etapas: uma pull request do space-quiz mesclada; uma execução bem-sucedida de Deploy website no GitHub Actions com as etapas Build website, Upload artifact e Deploy to Pages; e o Space Quiz publicado em yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

## Verifique a publicação

1. Acompanhe a execução de publicação do GitHub Actions até que ela seja concluída com sucesso.
2. Abra a URL informada e confirme se o Space Quiz carrega.

Se o agente não puder habilitar o Pages ou fazer o merge da pull request, siga as instruções específicas que ele fornecer. Não considere a tarefa concluída até que a publicação tenha sucesso e o site esteja acessível.

## Resumo e próximas etapas

O Space Quiz agora está disponível como um site publicado, e você verificou a implantação em vez de confiar apenas no relato do agente. Continue para a [Lição 13: Revisão e próximas etapas][next-lesson].

[next-lesson]: ../13-review/
