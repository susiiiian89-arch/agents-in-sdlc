---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

En el modo de agente, GitHub Copilot explora el código, propone cambios, edita archivos y ejecuta comandos de forma autónoma. Copilot Chat en VS Code muestra cada llamada a herramientas y cada edición en tiempo real. Puedes revisar los diffs, aceptar o rechazar cambios y orientar la conversación con prompts de seguimiento.

En este ejercicio supervisarás el trabajo del agente de accesibilidad, revisarás sus cambios y le pedirás que añada un control de modo claro junto al de alto contraste.

## Escenario

El agente personalizado de accesibilidad del [Ejercicio 4][previous-lesson] está implementando el modo de alto contraste. El equipo de diseño también ha pedido un modo claro. En vez de iniciar una conversación nueva, ampliarás el trabajo de la sesión actual.

## Supervisar el progreso de Copilot

La vista Copilot Chat muestra los pasos del agente: archivos que explora, comandos que ejecuta y cambios que propone. Cada edición incluye un diff y los controles **Keep** y **Undo**.

1. Vuelve a Copilot Chat desde el Ejercicio 4.
2. Mientras trabaja, recorre el chat para ver las lecturas, las ediciones y los comandos del terminal.
3. Cuando proponga una edición, selecciona **Keep** para aceptarla o **Undo** para revertirla y permitir otro intento.
4. Abre **Files changed**, en la parte superior del chat, para ver todos los archivos modificados.

> [!NOTE]
> Si una llamada a una herramienta requiere permiso, el agente se detendrá y mostrará **Allow**. Aprueba solo los comandos que te parezcan adecuados.

5. Espera a que termine el modo de alto contraste. El agente mostrará un mensaje con el resumen de los cambios.

## Orientar al agente para añadir el modo claro

Continúa la misma conversación para añadir un control de modo claro. La sesión conserva el contexto del trabajo anterior, incluidos los componentes, los estilos y el uso de almacenamiento local.

1. Envía este prompt de seguimiento en Copilot Chat:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. El agente retomará el trabajo y propondrá cambios para el modo claro.
3. Revisa cada edición y selecciona **Keep** o **Undo**.
4. Si el agente toma una dirección que no quieres, por ejemplo, refactoriza el panel de controles de una forma inadecuada, detenlo con **Stop** y envía un mensaje para aclarar lo que necesitas.

> [!TIP]
> Orientar una sesión en curso es más rápido que empezar de nuevo. Pide ajustes concretos, como simplificar el CSS, integrar el modo claro en el panel existente o añadir atajos de teclado.

## Revisar los cambios

Antes de confirmar el trabajo, revisa todo lo que haya modificado el agente.

1. Abre **Source Control** en VS Code.
2. Revisa la lista completa de archivos modificados; debería incluir componentes Astro, estilos y las pruebas relacionadas.
3. Inspecciona los diffs y confirma que se aplican las pautas de accesibilidad: atributos ARIA, navegación con teclado, HTML semántico y persistencia en almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios en el panel Source Control. Los publicarás en la siguiente lección.

## Resumen y siguientes pasos

Has supervisado una sesión en curso de Copilot Chat, revisado cada edición, aceptado o rechazado cambios y enviado prompts para ampliar el trabajo sin perder contexto.

En la [siguiente lección][next-lesson] publicarás el trabajo de accesibilidad como pull request y revisarás los cambios que haya generado el agente personalizado local.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Usar el modo de agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
