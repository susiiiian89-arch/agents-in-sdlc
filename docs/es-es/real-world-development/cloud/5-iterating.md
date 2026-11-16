---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestionar agentes][previous-lesson] |
|:--|

## Revisar el trabajo

Durante este laboratorio has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia y añadir funcionalidad: documentar el código, crear una funcionalidad de juegos relacionados para que el equipo de diseño pueda iterar sobre ella y añadir funciones de accesibilidad, incluidos controles para los modos de alto contraste y claro. Vamos a revisar los cambios y, si hace falta, dar indicaciones a Copilot para mejorarlos.

### Escenario

Como se ha señalado varias veces, los fundamentos del diseño de software y DevOps no cambian al incorporar IA generativa. Siempre hay que revisar el código generado y seguir el proceso habitual de DevOps. Con esto en mente, revisemos las sugerencias de GitHub Copilot para añadir documentación, crear la funcionalidad de juegos relacionados y añadir las funciones de accesibilidad antes de activar la revisión para el resto del equipo.

## Seguridad y GitHub Copilot cloud agent

Como Copilot cloud agent realiza sus tareas de forma asíncrona y sin supervisión, se han establecido ciertas restricciones de seguridad para que todo siga siendo seguro:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que utilizará para su código.
- Cloud agent se ejecuta en GitHub Actions y crea un entorno independiente y efímero para trabajar.
- Una persona debe aprobar los flujos de GitHub Actions antes de ejecutarlos.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisar la documentación generada

Empecemos por la primera pull request (PR) creada por GitHub Copilot cloud agent: la que añade documentación al código. Para esta tarea, utilizarás la interfaz estándar de PR de GitHub.com.

> [!NOTE]
> Al explorar la PR, es posible que veas un aviso de que el firewall bloquea a GitHub Copilot. Es **normal**: por defecto, Copilot tiene acceso limitado a recursos externos, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o deshabilitar el firewall de Copilot cloud agent][agent-firewall].

1. Vuelve a tu repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de pull requests.
3. Abre la PR titulada, por ejemplo, **Add missing documentation** u otro título más descriptivo.

> [!NOTE]
> Si Copilot sigue trabajando, la pull request incluirá la etiqueta **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos; puedes descansar un momento o repasar lo que has aprendido.

4. Cuando la pull request esté lista, selecciona **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que incluye los nuevos comentarios de documentación TSDoc y otros cambios de documentación. Los cambios exactos pueden variar.

    Al revisar los cambios, busca comentarios de documentación TSDoc, convenciones de TypeScript y encabezados de comentario. Proceden de los archivos de instrucciones personalizadas que revisaste al principio del Ejercicio 2.

6. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador de que hay flujos pendientes de aprobación.
8. Si hay flujos pendientes de aprobación, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Deberías ver los flujos en cola en la sección de comprobaciones de la pull request. Si todo va bien, las comprobaciones del proyecto para la única aplicación Astro se completarán correctamente. Puede tardar unos minutos.

## Solicitar cambios a GitHub Copilot

Trabajar con Copilot en una pull request no es un proceso unidireccional. Puedes mencionarlo en comentarios de la PR —igual que a otros miembros del equipo— o en comentarios insertados en el código. Copilot verá esos comentarios e iniciará otra sesión para resolverlos. Como los resultados no son deterministas, no podemos prescribir un texto concreto para solicitar cambios. Algunas ideas:

- Añadir encabezados de comentario al principio de cada archivo de código con una breve descripción de su contenido.
- Añadir comentarios TSDoc a los archivos TypeScript y Astro.
- Crear un README que describa la estructura de la aplicación Astro.

1. Añade un comentario para solicitar un cambio en la documentación generada y menciona a **@copilot**, como harías con cualquier otro usuario. Usa una de las ideas anteriores u otra sugerencia de documentación que quieras incorporar al código.
2. Selecciona **View Session** para ver el trabajo de Copilot. Observa cómo inicia una sesión nueva para aplicar los cambios.
3. Puedes seleccionar **Back to pull request** para volver a la pull request.

    ![Volver a la pull request](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine los cambios, debería aparecer un nuevo commit en la pull request.
5. Selecciona **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que estés conforme. Después, puedes marcar la PR como lista para revisión y fusionarla en la rama principal.

![Marcar la PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisar la funcionalidad de juegos relacionados

Volvamos a la PR que Copilot creó para resolver la incidencia sobre cómo mostrar juegos relacionados en la página de detalles.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título sea parecido a **Show related games on the game details page** u otro título más descriptivo.
4. Selecciona **Files changed** para revisar el código generado.
5. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
6. Deberías ver un indicador de que hay flujos pendientes de aprobación.
7. Si hay flujos pendientes de aprobación, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Deberías ver los flujos en cola en la sección de comprobaciones de la pull request. Si todo va bien, las comprobaciones del proyecto para la única aplicación Astro se completarán correctamente. Puede tardar unos minutos.
9. **Opcional:** también puedes cambiar a esta rama en tu Codespace para probar manualmente la funcionalidad de juegos relacionados. Abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo, **copilot/fix-8**:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

¡Copilot ha creado la funcionalidad de juegos relacionados! Como antes, puedes iterar con Copilot cloud agent para solicitar cambios. Por ejemplo, podrías pedirle que ajuste cuántos juegos relacionados se muestran o que añada encabezados de comentario y comentarios TSDoc. Recuerda que esta tarea se asignó **antes** de actualizar las instrucciones personalizadas. Puedes solicitar esos cambios con un comentario nuevo en la pestaña **Conversation**; Copilot lo verá e iniciará otra sesión.

## Revisar las funciones de accesibilidad

Por último, revisemos las funciones de accesibilidad implementadas con el agente personalizado de accesibilidad. Esta PR debe incluir tanto el modo de alto contraste que asignaste en el Ejercicio 3 como el modo claro solicitado en el centro de control del Ejercicio 4.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título sea parecido a **Add high contrast mode to website** u otro título más descriptivo.

> [!NOTE]
> Si Copilot sigue trabajando, la pull request incluirá la etiqueta **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos.

4. Selecciona **Files changed** para revisar el código generado.
5. Revisa la implementación y presta especial atención a:
   - Los componentes de interfaz para alternar entre modos.
   - El uso del almacenamiento local para conservar las preferencias del usuario.
   - Los cambios de CSS o estilos para los modos de alto contraste y claro.
   - Los atributos de accesibilidad, como las etiquetas ARIA y la navegación con teclado.
   - El código JavaScript/TypeScript que gestiona el cambio de modo.

6. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador de que hay flujos pendientes de aprobación.
8. Si hay flujos pendientes de aprobación, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Deberías ver los flujos en cola en la sección de comprobaciones de la pull request. Si todo va bien, las comprobaciones del proyecto para la única aplicación Astro se completarán correctamente. Puede tardar unos minutos.
10. **Opcional:** cambia a esta rama en tu Codespace para probar manualmente las funciones de accesibilidad. Abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama creada por Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    A continuación, inicia la aplicación y prueba los controles de alto contraste y modo claro en el navegador para comprobar que funcionan como esperas y que las preferencias se conservan al recargar la página.

Observa cómo el agente personalizado de accesibilidad ayudó a orientar a Copilot para implementar estas funciones según las buenas prácticas de accesibilidad. Si detectas problemas o posibles mejoras, puedes solicitar cambios mencionando a **@copilot** en un comentario, como hiciste con las PR anteriores.

## Ejercicio opcional: seguir delegando

Cloud agent funciona mejor cuando puedes asignarle elementos reales del backlog y centrarte en otra cosa. Para adquirir el hábito, crea algunas incidencias más en el repositorio y asígnaselas a Copilot. Algunas ideas:

- Crear un formulario de interés para mecenas en la página de detalles del juego.
- Implementar paginación en la página de la lista de juegos.
- Añadir validación de entradas y gestión de errores a los helpers de acceso a datos.

## Resumen

Has completado el itinerario de cloud agent. En estas lecciones:

- **Has inspeccionado los archivos de instrucciones personalizadas del repositorio** para reconocer su efecto en el resultado de cloud agent.
- **Has asignado incidencias a Copilot cloud agent** y has visto cómo prepara el entorno, planifica y trabaja de forma asíncrona.
- **Has creado y usado un agente personalizado** para implementar funciones de accesibilidad, como los modos de alto contraste y claro.
- **Has usado la página de agentes de Copilot como centro de control** para supervisar y orientar la sesión de accesibilidad en curso.
- **Has revisado e iterado sobre las pull requests de cloud agent**, mencionando a `@copilot` para solicitar cambios y aprobando flujos de trabajo.

## Repaso y siguientes pasos

Has completado el itinerario de cloud agent. Si quieres seguir explorando, los demás itinerarios complementan lo que has practicado:

- 🖥️ **[Itinerario de VS Code](../../vscode/)**: explora el modo de agente de Copilot Chat y la integración MCP directamente desde el IDE.
- 💻 **[Itinerario de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con los modos Plan y Autopilot, habilidades de agente, un agente de QA personalizado, Playwright MCP, Agent Merge y el puente opcional `/delegate` con cloud agent.

En tu propio repositorio, prueba estas ideas:

- Asigna a cloud agent una incidencia de refactorización o cobertura de pruebas.
- Crea un agente personalizado para otro ámbito.
- Configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Buenas prácticas para el flujo de configuración de Copilot cloud agent][cloud-agent-best-practices]
- [Configurar el firewall de Copilot cloud agent][agent-firewall]

---

| [← Lección anterior: Gestionar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
