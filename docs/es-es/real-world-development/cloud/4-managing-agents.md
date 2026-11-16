---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has pedido a Copilot cloud agent que se ocupe de tres tareas diferentes para mejorar la experiencia y añadir funcionalidad. Aunque cloud agent trabaja de forma autónoma y asíncrona, sigue siendo importante supervisar estas tareas.

Hay varias herramientas para gestionar las tareas asignadas, como la [página de agentes][agents-page] de GitHub.com. Desde este centro puedes ver las tareas de agentes con pull requests (PR) abiertas, explorar las operaciones realizadas y orientar una sesión en curso.

En esta lección explorarás la página de agentes para supervisar tareas y orientar una sesión en curso para solicitar funcionalidad adicional.

## Escenario

Después de asignar el modo de alto contraste, el equipo decidió que también sería buen momento para añadir un modo claro. Como ya se están actualizando los estilos y se añadirán controles de alternancia, quieres orientar al agente para que implemente ambos modos.

## Revisar las tareas de Copilot cloud agent

Consulta el estado de las tareas asignadas a Copilot cloud agent.

1. Abre la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Revisa las tareas del panel principal y del panel izquierdo. Deberías ver las tareas del laboratorio: actualizar la documentación, generar API para modificar juegos y añadir un modo de alto contraste.
3. Selecciona una tarea en curso y revisa las operaciones que ha realizado Copilot, como obtener el código del repositorio, preparar el entorno, configurar servidores MCP y completar la tarea.

> [!NOTE]
> Los pasos exactos varían según el estado del trabajo de Copilot y el enfoque que haya seguido.

4. Observa el panel de pull request de la derecha, que muestra la PR y los archivos modificados para facilitar la supervisión.

## Orientar a cloud agent

Pide a Copilot que añada el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título puede variar según el nombre que Copilot haya asignado y el estado del trabajo.

    ![Sesión de accesibilidad en mission control](../../../_images/ex6-accessibility-session.png)

2. Espera unos minutos hasta que termine la configuración y empiece el trabajo.
3. En el diálogo **Steer active session while Copilot is working**, escribe este prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Diálogo para orientar una tarea activa de cloud agent](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot confirma la solicitud y la incorpora al flujo de trabajo.

## Dejar que Copilot trabaje

Copilot continuará con la tarea actual e incorporará la nueva solicitud cuando termine el paso en curso. El proceso puede tardar varios minutos; puedes aprovechar ese tiempo para repasar lo aprendido.

## Resumen y siguientes pasos

Has explorado la página de agentes de Copilot, el centro de supervisión y orientación de las tareas de cloud agent. Permite seguir las tareas activas y completadas, revisar el trabajo y redirigir sesiones en curso. Has supervisado las tareas y solicitado funcionalidad adicional durante una sesión.

Ahora puedes [iterar sobre las pull requests de Copilot][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
