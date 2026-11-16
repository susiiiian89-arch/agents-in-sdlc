---
title: "Lección 8 - Crear y fusionar la PR de la funcionalidad"
description: "Revisa el filtrado, las instrucciones, la actualización de la habilidad, el perfil de QA y las pruebas; después, crea una PR y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de instrucciones, la actualización de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una rama. Es el momento de revisarlos juntos y abrir una pull request. La pull request (PR) de las valoraciones la fusionaste tú; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente, dividiríamos la funcionalidad, las actualizaciones de instrucciones, la actualización de la habilidad y el agente de QA en varias PR. Para agilizar el taller, has mantenido todo el trabajo de filtrado y del flujo de calidad en una sola conversación y rama, y lo incluirás en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una fusión;
- inspeccionarás todos los cambios de la funcionalidad y las pruebas de verificación;
- crearás la PR del filtrado;
- activarás Agent Merge únicamente después de revisar los cambios y confirmarás que la PR se ha fusionado.

## Escenario

Durante el flujo de trabajo de filtrado, has usado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere automatizar el trabajo restante de la PR sin quitar al desarrollador el control de autorización de la fusión.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una pull request. Cuando lo activas, Copilot resuelve los bloqueos de la PR: corrige las comprobaciones fallidas de integración continua (CI), responde a comentarios de revisión y vuelve a aplicar la base cuando hace falta. Después, activa la fusión automática de GitHub si el repositorio lo permite.

Hasta ahora has seleccionado tú **Merge pull request**. Agent Merge puede encargarse de esa responsabilidad. Revisa el trabajo y asegúrate de que está listo antes de activarlo.

## Usar Agent Merge para gestionar la PR

Con todo el código creado, revisémoslo, creemos la PR y dejemos que Agent Merge gestione el resto del proceso.

1. Vuelve a tu Codespace.
2. Abre el diálogo del agente con `/agent`.
3. Selecciona **Default** en la lista y pulsa <kbd>Enter</kbd>.
4. Crea una PR nueva con `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua de la PR. Cuando todas las comprobaciones terminen correctamente, realizará la fusión.
7. Comprueba que Copilot muestra un mensaje parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de fusión, la configuración del repositorio ni los permisos que falten. Si se bloquea, lee el motivo indicado y completa manualmente la fusión revisada cuando el repositorio lo permita.

## Resumen y siguientes pasos

Has automatizado varias partes del proceso de desarrollo: la generación del código, las pruebas y la validación, y ahora el proceso de pull request. En esta lección:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de una fusión;
- has inspeccionado todos los cambios de la funcionalidad y las pruebas de verificación;
- has creado la PR del filtrado;
- has activado Agent Merge después de revisar los cambios y confirmado que la PR se ha fusionado.

A continuación, [explorarás otros comandos de barra útiles de Copilot CLI][next-lesson] para gestionar el contexto, los modelos, el uso compartido y la delegación opcional en la nube.

## Recursos

- [Gestionar pull requests con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
