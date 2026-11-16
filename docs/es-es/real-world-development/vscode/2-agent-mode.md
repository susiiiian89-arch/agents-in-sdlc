---
title: "Ejercicio 2 - Añadir funcionalidad con el modo de agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y operaciones como ejecutar pruebas. Como desarrollador, normalmente tienes que localizar todos los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo te has dejado y volver a modificarlo… La lista continúa.

Aquí es donde entra en juego el modo de agente de Copilot.

El modo de agente de Copilot está diseñado para trabajar de forma más autónoma en el IDE. Se comporta de manera parecida a un desarrollador: primero explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentra. Vamos a ver cómo usarlo para añadir funcionalidad al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo de agente y cloud agent están diseñados para experiencias distintas. El modo de agente trabaja en tu IDE y permite ciclos de respuesta rápidos e interacción. Cloud agent está pensado para trabajar de forma asíncrona como un compañero del equipo e interactuar contigo mediante incidencias y pull requests.

En este ejercicio aprenderás:

- cómo el modo de agente de Copilot puede explorar el proyecto, identificar los archivos pertinentes y coordinar cambios;
- cómo puede implementar funcionalidades en la interfaz de usuario y la capa de datos;
- cómo revisar los cambios y las pruebas que genera antes de incorporarlos al código.

## Escenario

A medida que crece la lista de juegos, quieres permitir que los usuarios filtren por categoría y editorial. En el ejercicio anterior ya añadiste un helper para las editoriales; ahora completarás con Copilot el resto del trabajo en la capa de datos, la interfaz y las pruebas.

## Ejecutar el sitio web de Tailspin Toys

Antes de hacer cambios, explora el sitio web de Tailspin Toys para conocer su funcionalidad actual.

El sitio web es una plataforma de financiación colectiva de juegos de mesa con una temática de desarrollo de software. Permite mostrar juegos y sus detalles. Es una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local desde su frontmatter mediante Drizzle ORM; no hay una API de backend independiente ni un framework de interfaz en el cliente. Los helpers reutilizables para acceder a los datos están en `src/lib/`, y la interactividad se implementa con un pequeño elemento `<script>` de Astro y las API estándar del DOM.

### Iniciar el sitio web

Para facilitar el inicio del sitio, se ha proporcionado la tarea `npm run dev`, que arranca el único servidor de desarrollo de Astro. Puedes ejecutarla en GitHub Codespaces siguiendo estos pasos:

1. Vuelve a tu Codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre un terminal nuevo en el Codespace con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea y cambia a una rama nueva para este trabajo de filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Ejecuta este comando para iniciar el sitio web:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, aparecerá un mensaje con la URL, parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir una ventana del navegador en `http://localhost:4321`, ciérralo seleccionando la **x**.

5. Abre el sitio manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando en el terminal la dirección del servidor Astro: `http://localhost:4321`.

> [!NOTE]
> En un Codespace, al seleccionar el enlace localhost en el terminal se redirige automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Se trata de un túnel privado a tu Codespace, que ahora aloja el servidor web.

### Explorar el sitio web

Cuando el sitio esté en marcha, explora sus funciones principales:

- **Página de inicio:** muestra una lista de juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego:** al seleccionar un juego, se abre una página con más información, como el título, la descripción, la editorial y la categoría.

## Explorar el backlog con Copilot

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve a tu Codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; se indicarán cuando corresponda.

La implementación inicial del sitio funciona, pero queremos mejorarla con nuevas funciones. Empecemos por revisar el backlog. Al configurar el proyecto a partir de la plantilla, se crearon automáticamente varias incidencias; pide a GitHub Copilot que te las muestre.

1. En el selector de agentes de la vista Chat, selecciona **Agent**. El agente **Agent** planifica e implementa cambios de forma autónoma en varios archivos, ejecuta comandos de terminal e invoca herramientas.

   ![Captura de pantalla del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores de este taller no muestran preferencia por un modelo concreto. Al preparar el taller usamos Claude Sonnet 4.5 y lo incluimos en las instrucciones para que las sugerencias de código sean relativamente uniformes y la experiencia sea coherente. Sin embargo, como los modelos de lenguaje de gran tamaño son probabilísticos, las sugerencias pueden diferir de las descritas en el taller. Es normal y esperable.

> [!NOTE]
> Debido a la naturaleza probabilística de los modelos de lenguaje de gran tamaño, Copilot puede utilizar un comando MCP distinto y aun así completar la tarea.

3. Envía a Copilot esta solicitud sobre las incidencias pendientes:

   ```plaintext
   Muéstrame el backlog de elementos de mi repositorio de GitHub. Ayúdame a priorizarlos según su utilidad para el usuario.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera todas las incidencias.
5. Revisa la lista generada.

Fíjate en que Copilot ha priorizado los elementos según los que considera más útiles para el usuario.

## Revisar los archivos de instrucciones

Antes de pedirle al agente que genere código, conviene revisar el archivo de instrucciones que proporcionará contexto para su trabajo. Usarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo abordar la incorporación de funcionalidad al sitio.

1. En tu Codespace, abre `.github/instructions/ui.instructions.md`.
2. Observa las pautas generales para añadir funcionalidad. Incluyen:
   - Una descripción general de la arquitectura.
   - Principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - Enlaces a archivos de instrucciones específicos para distintos tipos de archivo, entre ellos:
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> Los archivos de instrucciones pueden hacer referencia a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas a la ubicación del archivo de instrucciones. Esto permite reutilizar instrucciones, dividirlas en partes más manejables y proporcionar ejemplos y plantillas.

## Implementar la funcionalidad de filtrado

Para completar el filtrado, habrá que realizar al menos tres cambios independientes en la aplicación:

- Añadir o mejorar la lógica de filtrado restante en la capa de datos (`src/lib/`).
- Añadir o actualizar pruebas para el comportamiento del filtrado.
- Actualizar la página de lista de juegos para incorporar la interfaz de filtrado.

Además, las pruebas deben ejecutarse y pasar antes de integrar los cambios en el código. ¡El modo de agente de Copilot puede encargarse de estas tareas! Vamos a añadir la funcionalidad.

1. Puedes continuar la conversación actual con Copilot o iniciar otra seleccionando **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui** para añadir el archivo de instrucciones.

   ![Captura de pantalla de la selección del archivo de instrucciones de UI](../../../_images/ex3-select-instructions-file.png)

3. Comprueba que **Agent** sigue seleccionado en el selector de agentes de la vista Chat.

   ![Captura de pantalla del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Comprueba que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la funcionalidad según la incidencia relacionada del backlog con esta solicitud:

   ```plaintext
   Actualiza el sitio para incluir el filtrado por editorial y categoría según los requisitos de la incidencia relacionada del backlog de GitHub. Ya existe un helper para las editoriales del ejercicio anterior; consérvalo y mejóralo si hace falta mientras completas el resto de la capa de datos, la interfaz y las pruebas. Asegúrate de que todas las pruebas pasan antes de terminar. El servidor ya está en marcha, así que no hace falta iniciarlo.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos relacionados con la funcionalidad. Deberías ver que encuentra los helpers de la capa de datos, la interfaz y las pruebas. Después, modifica los archivos y ejecuta las pruebas.

   ![Captura de pantalla de Copilot explorando los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. Según la complejidad de la tarea y del código base, puede tardar unos minutos. Durante el proceso quizá aparezcan los botones **Keep** y **Undo** en el editor de código. Cuando termine, tendrás la opción **Keep** o **Undo** para todos los cambios; no tienes que seleccionar ninguna mientras siga trabajando.

7. Cuando Copilot lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Captura de pantalla de un diálogo de Copilot Chat que pide confirmación para ejecutar las pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Es posible que haya pausas e incluso que algunas pruebas fallen durante el proceso. No pasa nada: Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea sin detectar errores.

   ![Captura de pantalla de una sesión completa de Copilot Agent Mode](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Examina el código generado para detectar posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador donde está abierto el sitio y explora la nueva funcionalidad.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y siguientes pasos

¡Enhorabuena! En este ejercicio has aprendido a usar el modo de agente de GitHub Copilot para añadir funcionalidades al sitio web Tailspin Toys:

- El modo de agente de GitHub Copilot puede implementar nuevas funciones en la interfaz y la capa de datos.
- El modo de agente de Copilot puede explorar el proyecto, identificar los archivos pertinentes y coordinar cambios.
- Puedes revisar los cambios y las pruebas que genera antes de incorporarlos al código.

Ahora [prueba la funcionalidad con el servidor MCP de Playwright][next-lesson] y crea una pull request.

### Ejercicio de exploración adicional: implementar paginación

A medida que crece la lista de juegos, será necesario habilitar la paginación. Aplica lo aprendido en este ejercicio y pide a Copilot que actualice el sitio para implementarla. Al pensar en el código, ten en cuenta lo siguiente:

- Sigue las buenas prácticas existentes, incluido el uso de los archivos de instrucciones disponibles.
- Decide cómo quieres implementar la paginación: por ejemplo, si el usuario podrá elegir el tamaño de página o si será fijo.
- Al redactar la solicitud, proporciona a Copilot las indicaciones necesarias para crear la implementación que quieres.
- Puede que tengas que iterar con GitHub Copilot, solicitar cambios y proporcionar contexto. ¡Es el flujo normal al trabajar con Copilot!

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo usarlos][choose-mode]
- [Modo de agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
