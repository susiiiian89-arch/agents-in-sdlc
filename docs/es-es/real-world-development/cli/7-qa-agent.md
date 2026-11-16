---
title: "Lección 7 - Crear y usar un agente de QA"
description: "Crea un agente de QA que combine los requisitos de la incidencia, la habilidad quality-checks y Playwright MCP."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Has usado la habilidad `quality-checks` para ejecutar comprobaciones automatizadas y Playwright MCP para observar en un navegador la experiencia del filtrado. Ahora combinarás esas capacidades en un agente personalizado con un proceso de QA bien definido.

En esta lección:

- explorarás cómo trabaja un agente personalizado con instrucciones, habilidades y herramientas MCP;
- crearás y revisarás un perfil reutilizable de control de calidad;
- seleccionarás el agente de QA y revisarás sus conclusiones sobre la incidencia de filtrado.

## Escenario

Antes de abrir una pull request (PR), Tailspin Toys quiere revisar de forma coherente los requisitos, la calidad del código, las comprobaciones automatizadas, la cobertura de pruebas y el comportamiento en el navegador. Un agente personalizado puede coordinar el proceso de QA y generar un informe reutilizable.

## ¿Qué es un agente personalizado?

Un agente personalizado es una versión especializada de Copilot definida en un perfil Markdown. El perfil describe el propósito, las instrucciones y las herramientas disponibles para el agente. En este taller, definirás el rol de QA en `.github/agents/qa.agent.md` y lo seleccionarás en Copilot CLI.

Las personalizaciones que has usado tienen objetivos distintos. Las instrucciones del repositorio describen los estándares del equipo. La habilidad `quality-checks` agrupa comprobaciones repetibles. Playwright MCP proporciona herramientas de navegador. El perfil de QA indica a Copilot cómo usar esas capacidades para evaluar los requisitos e informar de los resultados; no las sustituye ni requiere otra conversación.

## Crear el perfil de QA

Antes de abrir la PR de la funcionalidad, pide a Copilot que cree un perfil de QA reutilizable. El perfil definirá las comprobaciones y los límites que debe respetar.

1. Vuelve a tu Codespace y confirma que la conversación de filtrado está en modo Interactive.
2. Pide al agente predeterminado que cree el agente personalizado:

   ```plaintext
   Crea un agente personalizado llamado QA en .github/agents/qa.agent.md. Debe comprobar las funcionalidades frente a sus incidencias y los requisitos acordados, seguir las instrucciones del repositorio, ejecutar la habilidad quality-checks, usar Playwright MCP para verificar el comportamiento y añadir pruebas si falta cobertura.

   Debe informar del estado de cada requisito como superado, fallido o bloqueado, con pruebas que respalden la conclusión. Debe pedir permiso antes de modificar el código de implementación y no debe confirmar cambios ni abrir pull requests.

   Por ahora, crea solo el perfil para que pueda revisarlo.
   ```

## Inspeccionar el perfil

Antes de usar el agente, revisa su perfil para confirmar que Copilot ha capturado el flujo de QA y sus límites. Así evitarás que un perfil incompleto o demasiado amplio modifique una funcionalidad que solo querías verificar.

1. Escribe `/diff` y abre `.github/agents/qa.agent.md`.
2. Lee el frontmatter. `description` es obligatorio; `name` es opcional, aunque proporciona un nombre claro en la interfaz.
3. Revisa las instrucciones del perfil y confirma que QA empieza por los requisitos, sigue las instrucciones del repositorio, ejecuta `quality-checks` y usa Playwright MCP.
4. Confirma que QA presenta pruebas, pide permiso antes de cambiar el código de implementación y no confirma cambios ni abre pull requests.
5. Si falta alguna responsabilidad o límite, pide al agente predeterminado que revise el perfil antes de continuar.

## Ejecutar QA para la incidencia

Copilot CLI carga los agentes del proyecto al iniciar una conversación. Después de crear el perfil, retoma la misma conversación de filtrado y selecciona QA para que pueda usar la incidencia y las decisiones de planificación que ya están en el contexto.

1. Activa el agente con este comando:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como acabas de crear el agente, quizá no aparezca en la lista de agentes disponibles. El comando anterior lo activará igualmente.

2. Envía este prompt para pedir al agente de QA que revise la funcionalidad:

   ```plaintext
   Revisa la funcionalidad de filtrado según la incidencia y las decisiones del plan. ¿Está lista para una pull request?
   ```

3. El agente de QA empezará a trabajar.
4. Cuando termine, lee el informe.

## Resumen y siguientes pasos

Has añadido un rol especializado y reutilizable al flujo de trabajo y has revisado su trabajo. En esta lección:

- has explorado cómo trabaja un agente personalizado con instrucciones, habilidades y herramientas MCP;
- has creado e inspeccionado un perfil reutilizable de control de calidad;
- has seleccionado el agente de QA y revisado sus conclusiones sobre la incidencia de filtrado.

Ya tienes lista para revisar la implementación, la actualización de la habilidad, el perfil de QA, las pruebas y el informe de verificación. A continuación, [los reunirás en una PR de la funcionalidad y usarás Agent Merge][next-lesson].

## Recursos

- [Crear agentes personalizados para Copilot CLI][create-agents]
- [Configuración de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
