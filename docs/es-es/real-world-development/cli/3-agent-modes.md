---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Usa Plan para acordar un enfoque, Autopilot para crear el filtrado a partir de una incidencia e Interactive para revisar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Empezamos añadiendo una pequeña funcionalidad al proyecto. Sin embargo, los cambios más grandes requieren un proceso más sólido. Por suerte, GitHub Copilot CLI está diseñado para trabajar con el flujo existente de una organización y ayudarnos a crear lo adecuado de la manera adecuada. Esta es la primera de varias lecciones en las que seguirás un proceso de desarrollo habitual basado en agentes: partirás de una incidencia para crear una funcionalidad, comprobarás que el código es válido y que se comporta como esperas y, por último, la fusionarás correctamente en el proyecto.

> [!NOTE]
> A medida que avances en el flujo de trabajo de esta funcionalidad, usarás la misma conversación y rama. Normalmente se usarían ramas o PR diferentes para los distintos tipos de archivo, pero haremos una excepción para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una conversación de Copilot nueva desde una incidencia de GitHub;
- definirás los requisitos en el modo Plan;
- implementarás la funcionalidad con el modo Autopilot;
- revisarás el código;
- validarás la funcionalidad manualmente en un navegador reenviado.

Al continuar con esta funcionalidad, actualizarás las instrucciones del repositorio, personalizarás la habilidad existente `quality-checks`, añadirás validación con MCP, crearás un agente de QA y abrirás la PR de la funcionalidad.

## Escenario

El catálogo de Tailspin Toys está creciendo y los visitantes necesitan filtrar los juegos por categoría y editorial. La incidencia del backlog describe la funcionalidad, pero antes de programar hay que acordar detalles como la combinación de categorías. Usarás Plan para resolver esas decisiones y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

Incorporar agentes de programación con IA al flujo de desarrollo no cambia sus fundamentos; de hecho, los hace aún más importantes. La mayoría de los desarrolladores siguen un proceso parecido a este:

1. Partir de una incidencia registrada que describa el trabajo.
2. Planificar lo que se debe crear.
3. Crear y revisar el código.
4. Ejecutar pruebas para validar el código.
5. Validar manualmente la nueva funcionalidad.
6. Crear una pull request (PR).
7. Cuando se haya revisado el código y se hayan superado las comprobaciones de integración continua, fusionarlo.

> [!NOTE]
> Los detalles exactos varían según el equipo y la organización, pero la mayoría de los procesos siguen alguna variante de estos pasos.

Al ceñirte a este enfoque, te aseguras de que el código generado por IA cumpla los requisitos y pase por el mismo proceso de revisión que el código escrito manualmente.

## Modos de conversación

El **modo de conversación** controla la autonomía del agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar de modo:

- **Interactive**: trabajas con el agente. Este propone cambios y espera tu respuesta antes de continuar.
- **Plan**: el agente crea primero un plan y no puede editar los archivos del proyecto.
- **Autopilot**: el agente trabaja de forma autónoma: escribe código, ejecuta pruebas e itera hasta completar la tarea.

Empieza en Plan, revisa el plan y, después, usa Autopilot para implementarlo.

## Empezar por la incidencia

Antes de comenzar a trabajar en el filtrado, vuelve a tu Codespace y comprueba que el repositorio y el terminal están preparados.

1. Vuelve a tu Codespace. Si está detenido, reinícialo.
2. Confirma que la PR de las valoraciones con estrellas está fusionada.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama para el filtrado:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa <kbd>Tab</kbd> dos veces para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Usa las teclas de dirección para resaltar la incidencia titulada **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia al prompt y volver a la pestaña **Session**.

Observa que el prompt ahora empieza por `#7` (o un número parecido). El carácter `#` permite añadir al contexto una incidencia o una pull request de GitHub.

## Planificar el filtrado

La planificación permite definir cómo se implementará una funcionalidad o se realizará una tarea antes de asignársela a Copilot. Para cualquier trabajo complejo, conviene dedicar un momento a planificar. Cambia al modo Plan y pide a Copilot que cree el plan.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Plan. Confirma que el indicador debajo del prompt muestra **Plan**.
2. Después de la referencia a la incidencia que añadiste en el paso anterior, escribe el siguiente prompt:

   ```plaintext
   Crea un plan para implementar esta funcionalidad.
   ```

Copilot empezará a elaborar el plan. Primero explorará el proyecto y, después, decidirá cuál es el mejor enfoque.

3. Es posible que Copilot te pregunte cómo debería funcionar el filtrado. Responde según tus preferencias; no hay respuestas incorrectas.
4. Cuando termine el plan, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandirlo.
5. Desplázate por el plan para revisarlo.
6. Pide a Copilot que revise cualquier parte que no coincida con tus decisiones.

## Aprobar Autopilot

Con el plan escrito y revisado, es el momento de implementarlo. Autopilot permite a Copilot iterar sobre el problema hasta que considera que está resuelto.

1. Selecciona **Accept plan and build on autopilot (recommended)** o la opción equivalente de tu versión.
2. Confirma que el indicador debajo del prompt muestra **Autopilot**.
3. Observa cómo Copilot sigue el plan, genera código y ejecuta pruebas.

> [!NOTE]
> Al aprobar, la implementación puede empezar inmediatamente; revisa primero el plan. Si Copilot informa de dependencias ausentes o conflictos de puertos, resuelve el problema de configuración antes de dar por terminadas las comprobaciones.

## Revisar y verificar la implementación

Una vez generado el código, revísalo antes de fusionarlo, igual que harías con cualquier otro cambio. Revisaremos el código y ejecutaremos el sitio para comprobar el resultado.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Interactive. Confirma que el indicador ya no muestra **Plan** ni **Autopilot**.
2. Escribe `/diff` e inspecciona la implementación del filtrado y sus pruebas.
3. Compara el resultado con la incidencia y las decisiones de planificación.
4. Cuando termines la revisión, pulsa <kbd>Esc</kbd> para salir de la vista del diff.
5. Revisa los resultados de las comprobaciones del proyecto y pide a Copilot que corrija los errores.

## Explorar la nueva funcionalidad

El código parece correcto, pero ¿se ejecuta? Inicia la aplicación como antes y ábrela mediante el puerto reenviado de Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Inicia la aplicación para que pueda probar la funcionalidad de filtrado en mi navegador. Dime la URL y deja el servidor en ejecución.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba los filtros de categoría y editorial, así como las combinaciones acordadas en el plan.
4. Confirma que el comportamiento de restablecimiento y de resultados vacíos coincide con la incidencia y tus decisiones.
5. Vuelve a tu Codespace y pide a Copilot que detenga el servidor de desarrollo que inició.

## Resumen y siguientes pasos

Has usado varios modos de conversación para crear y revisar una funcionalidad. En esta lección:

- has iniciado una conversación nueva de Copilot desde una incidencia de GitHub;
- has definido los requisitos en el modo Plan;
- has implementado la funcionalidad con el modo Autopilot;
- has revisado el código;
- has validado manualmente la funcionalidad en un navegador reenviado.

A continuación, profundizarás en cómo generar código de acuerdo con las prácticas documentadas mediante [instrucciones personalizadas][next-lesson].

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
