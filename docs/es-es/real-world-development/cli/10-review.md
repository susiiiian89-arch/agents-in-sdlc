---
title: "Lección 10 - Repaso y siguientes pasos"
description: "Recapitula el flujo de trabajo de Copilot CLI, las dos pull requests, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has usado GitHub Copilot CLI a lo largo de un flujo continuo de Tailspin Toys. Has:

- preparado un Codespace, instalado Copilot CLI, explorado el proyecto y encontrado la incidencia de filtrado del backlog;
- añadido valoraciones con estrellas, revisado el resultado en un navegador reenviado y fusionado manualmente tu primera pull request (PR);
- partido de la incidencia de filtrado, definido el enfoque en Plan, implementado con Autopilot y revisado con Interactive;
- orientado al agente con instrucciones personalizadas, personalizado la habilidad existente `quality-checks` y usado esa habilidad para ejecutar pruebas unitarias, lint y comprobaciones de tipos;
- añadido el servidor Model Context Protocol (MCP) de Playwright y usado el navegador para explorar el filtrado;
- creado y seleccionado un agente personalizado de control de calidad (QA) para evaluar los requisitos, la cobertura, los resultados de la habilidad y las pruebas obtenidas en el navegador;
- revisado el cambio completo de filtrado y autorizado Agent Merge para la PR correspondiente;
- explorado comandos de barra para el contexto, los modelos, el uso compartido y la delegación opcional en la nube.

## Qué has publicado

El taller tiene dos hitos de PR, cada uno en su propia rama creada desde `main` actualizado:

1. **Valoraciones con estrellas:** mostrar el valor existente de `starRating` y un estado explícito para los juegos sin valorar en las tarjetas.
2. **Filtrado y flujo de calidad:** implementar el filtrado, actualizar las instrucciones y aplicarlas a la funcionalidad, personalizar el informe `quality-checks`, crear un perfil de QA e incluir las pruebas asociadas.

Desde la planificación del filtrado hasta la creación de su PR, has usado la misma conversación y rama. Para agilizar el taller, agrupamos todo ese trabajo en una PR.

## Distintos tipos de verificación

Has comprobado el código de varias formas: con pruebas automatizadas, examinándolo en el navegador y dejando que Copilot explore el sitio mediante MCP. La habilidad `quality-checks` ejecutó las pruebas unitarias, el lint y las comprobaciones de tipos, y los presentó con el nuevo formato. Antes de abrir la PR, QA reunió esos resultados con una revisión de los requisitos y de la cobertura de pruebas.

Las pruebas nuevas deben cubrir carencias reales; si QA concluye correctamente que no hacen falta pruebas, también puede ser válido. Revisa el código y las pruebas antes de autorizar la fusión y actualiza las pruebas afectadas después de hacer cambios.

## Buenas prácticas

El contexto y las herramientas que proporcionas influyen en el trabajo de Copilot. En este taller has actualizado instrucciones, personalizado una habilidad, creado un perfil de QA y configurado un servidor MCP. Reutiliza estas personalizaciones en distintas conversaciones y ajústalas a las necesidades del equipo. Las instrucciones establecen estándares; las habilidades describen tareas repetibles; los agentes personalizados definen roles especializados; y los servidores MCP conectan herramientas externas. Revisa los cambios y los resultados de las herramientas, no solo el resumen del agente.

Elige el **modo y el modelo** según la tarea. Usa **Plan** para pensar el enfoque antes de empezar, **Interactive** para mantenerte al tanto de los cambios concretos y **Autopilot** para tareas bien delimitadas. Elige un modelo más rápido para ediciones rutinarias y otro más capaz para trabajos complejos.

El contexto sigue siendo tan importante como la infraestructura. Describir con claridad *qué* quieres crear, *por qué* y *cómo* influye de forma importante en el resultado.

## Más cosas por explorar

Has aprendido el flujo principal. También puedes probar estas funciones de CLI:

- `/review` para pedir al agente de revisión de código que analice los cambios;
- `/rubber-duck` para explicar un problema y obtener otra perspectiva;
- `/fleet` para coordinar tareas independientes en paralelo;
- `/worktree` para aislar otra tarea;
- `/delegate` para enviar una tarea a Copilot cloud agent.

## Siguientes pasos

La mejor forma de mejorar con cualquier herramienta es seguir usándola: en código de producción, en proyectos personales o en esa pequeña aplicación que hace años que quieres crear. Comparte lo aprendido con el equipo y aprende también de los demás. Y, como siempre, consulta la documentación.

Para ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Foundry Backer Concierge][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación GitHub Copilot][app] o el [taller de Copilot cloud agent][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar pull requests con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
