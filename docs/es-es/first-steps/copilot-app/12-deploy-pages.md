---
title: "Lección 12 - Publicar el sitio con GitHub Pages"
description: "Usa la aplicación GitHub Copilot para configurar un flujo de trabajo de Pages, revisar su pull request y verificar que Space Quiz está publicado."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Publica Space Quiz con GitHub Pages para que todo el mundo pueda acceder. Copilot puede preparar el flujo de trabajo y guiarte en el despliegue, pero tú debes revisar los cambios y confirmar que el sitio está disponible.

En esta lección:

- pedirás a Copilot que configure un despliegue de GitHub Pages;
- revisarás el flujo de trabajo y la pull request antes de fusionarla;
- seguirás el despliegue y comprobarás el sitio publicado.

## Escenario

Space Quiz ya está en un repositorio de GitHub, pero hace falta una forma sencilla de probarlo. Publicar el sitio de un solo archivo con GitHub Pages lo hace accesible mediante una URL pública, sin añadir un servidor ni dependencias de ejecución.

## Pedir a Copilot que prepare el despliegue

> [!WARNING]
> Crear una pull request puede confirmar y enviar código. Revisa la configuración de aprobaciones y el repositorio de destino antes de enviar este prompt. Si has seleccionado **Approve all**, no esperes otro aviso de confirmación.

En la sesión de la aplicación Copilot para `space-quiz`, envía este prompt:

```plaintext
Prepare a GitHub Pages deployment for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages after changes reach the default branch. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and report its link and any required Pages settings or permissions. Do not merge, enable auto-merge, change Pages settings, or deploy. Stop and wait for my review.
```

## Revisar el flujo de trabajo y la pull request

1. Confirma que el archivo README solo se añade si el repositorio aún no tiene uno.
2. Revisa los cambios de la pull request, incluidos los pasos de compilación y despliegue, los desencadenadores y los permisos del flujo.
3. Confirma que el flujo despliega desde la rama predeterminada, no desde una pull request sin fusionar.
4. Solicita las correcciones necesarias y revisa el diff actualizado. Espera a que se superen las comprobaciones obligatorias de la pull request antes de autorizar la fusión.

## Autorizar la fusión y el despliegue

> [!WARNING]
> El siguiente prompt autoriza al agente a fusionar la pull request revisada, cambiar la configuración de Pages del repositorio y publicar el sitio. Envíalo únicamente después de revisar los cambios y confirmar el repositorio y la pull request. Según la configuración de aprobaciones, estas acciones podrían realizarse sin otro aviso.

Sustituye `<PR_NUMBER>` por el número de la pull request que has revisado y envía este prompt por separado:

```plaintext
I have reviewed pull request <PR_NUMBER> in this repository and authorize its merge and deployment. Enable GitHub Pages using GitHub Actions, merge that pull request only after required checks pass, and monitor the deployment until it succeeds. Report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![Diagrama de tres fases: una pull request de space-quiz fusionada; una ejecución correcta de Deploy website en GitHub Actions con los pasos Build website, Upload artifact y Deploy to Pages; y Space Quiz publicado en yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

## Verificar el despliegue

1. Sigue la ejecución de despliegue de GitHub Actions hasta que termine correctamente.
2. Abre la URL indicada y confirma que se carga Space Quiz.

Si el agente no puede habilitar Pages o fusionar la pull request, sigue las instrucciones concretas que te proporcione. No des la tarea por terminada hasta que el despliegue finalice correctamente y el sitio publicado se cargue.

## Resumen y siguientes pasos

Space Quiz ya está disponible y has verificado el despliegue en lugar de confiar únicamente en el informe del agente. Continúa con la [Lección 13: Repaso y siguientes pasos][next-lesson].

[next-lesson]: ../13-review/
