---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo de agente →][next-lesson]

El contexto es clave cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta o Copilot necesita información adicional, asegúrate de que pueda acceder a ella. Los [archivos de instrucciones][instruction-files] indican cómo generar el código y permiten especificar no solo *qué* quieres, sino también *cómo* debe hacerse.

En este ejercicio:

- explorarás cómo el contexto específico del proyecto, las pautas de código y los estándares de documentación llegan a Copilot mediante instrucciones personalizadas del repositorio y archivos de instrucciones con ámbito de ruta;
- generarás la primera parte de los datos del filtrado (un helper de editoriales) con las instrucciones *actuales*;
- añadirás un estándar general nuevo a `.github/copilot-instructions.md`;
- volverás a ejecutar el mismo prompt y observarás cómo el código generado sigue el estándar nuevo;
- confirmarás en `main` la actualización de instrucciones y la base del filtrado para que Copilot use las nuevas pautas en el ejercicio siguiente.

> [!CAUTION]
> El código generado puede no seguir todos los estándares: Copilot no es determinista. El objetivo es observar la tendencia del cambio después de actualizar las instrucciones, no obtener una salida idéntica carácter por carácter.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene varias pautas de desarrollo:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz debe usar el modo oscuro y tener un aspecto moderno.
- El código debe incluir comentarios de documentación TSDoc.
- Cada archivo debe empezar con un bloque de comentarios que describa su contenido.

Los archivos de instrucciones ayudarán a Copilot a realizar las tareas de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas te permiten proporcionar contexto y preferencias a Copilot para que entienda mejor tu estilo de desarrollo y tus requisitos. Esta función puede ayudarte a orientar a Copilot para que ofrezca sugerencias y fragmentos de código más útiles. Puedes especificar las convenciones de código y bibliotecas que prefieres, e incluso los tipos de comentarios que quieres incluir. Puedes crear instrucciones para todo el repositorio o para tipos de archivo concretos y aportar así contexto para una tarea.

- `.github/copilot-instructions.md` se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información general del proyecto, como la pila tecnológica, una descripción, buenas prácticas y directrices globales.
- `.github/instructions/*.instructions.md` permite definir instrucciones para tipos de archivo o tareas concretos, como TypeScript, Astro, componentes de interfaz o pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se usan para generar código en Copilot Chat, no para las finalizaciones de código ni las sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y Copilot cloud agent usan las instrucciones del repositorio y los archivos `*.instructions.md` con frontmatter `applyTo`.
>
> Copilot también [admite otros formatos][custom-instructions-support], como AGENTS.md y CLAUDE.md.

### Buenas prácticas para gestionar archivos de instrucciones

Una conversación completa sobre la creación de archivos de instrucciones queda fuera del alcance del taller. Sin embargo, los ejemplos del proyecto muestran un enfoque representativo. A grandes rasgos:

- Mantén `copilot-instructions.md` centrado en directrices de ámbito general, como una descripción del proyecto, su estructura y los estándares globales de código.
- Usa `*.instructions.md` para proporcionar directrices específicas de tipos de archivo (pruebas unitarias, componentes Astro o la capa de datos) o de tareas.
- Usa lenguaje natural, expresa las directrices con claridad y proporciona ejemplos de cómo debería ser el código y de cómo no debería ser.

No existe una sola forma correcta de crear estos archivos: experimenta para descubrir qué funciona en tu proyecto.

> [!TIP]
> Explora los archivos de instrucciones del proyecto, incluidos los de [interfaz][ui-instructions] y [Astro][astro-instructions]. Copilot también puede generarlos; cada entorno ofrece una experiencia diferente, como **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. Para encontrar plantillas, visita [awesome-copilot][awesome-copilot].

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar los archivos de instrucciones personalizadas de este proyecto

Abre estos archivos en el editor o en la interfaz web de GitHub:

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la descripción del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa las directrices anidadas de **GitHub Actions Workflows**. Se aplican a cualquier interacción con Copilot.
3. Explora la carpeta `.github/instructions`. Hay instrucciones para archivos Astro, la capa de datos de Drizzle, pruebas y más.
4. Abre `.github/instructions/unit-tests.instructions.md`. El campo `applyTo` de la parte superior define un glob (relativo a la raíz del repositorio) que determina los archivos aplicables. Por ejemplo, coincide con los archivos de prueba TypeScript que se ajusten a `**/*.test.ts`.
5. Revisa las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio. Esto permite dividir conjuntos grandes de instrucciones en archivos pequeños y reutilizables, y mostrar a Copilot ejemplos que puede seguir.

> [!NOTE]
> **Code formatting requirements** documenta los estándares de código, pero todavía no exige documentación dentro del código. Añadirás reglas para comentarios TSDoc y encabezados de archivo.

## Crear una rama para los cambios

Sigamos algunas buenas prácticas y creemos una rama para los cambios.

1. Vuelve al codespace del ejercicio anterior.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama nueva y cambia a ella:

```bash
git checkout -b custom-instructions
```

## Usar Copilot Chat *antes* de actualizar las instrucciones

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso lo indicará cuando sea relevante.

Para ver el efecto de las instrucciones personalizadas, empieza enviando un prompt mientras el archivo de instrucciones actual sigue vigente. Después, actualizarás el archivo y volverás a enviar el mismo prompt para comparar el resultado.

1. Cierra las pestañas de ejercicios anteriores para que Copilot solo tenga el contexto necesario.
2. Abre `src/lib/publishers.ts` para indicar dónde debe estar el helper.
3. Selecciona **Agent** en el selector de agentes de Chat.

   ![Selector de agente de la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Envía este prompt:

   ```plaintext
   Crea o actualiza src/lib/publishers.ts con un helper de acceso a datos que devuelva la lista de todas las editoriales, con el nombre y el id de cada una. Aplica los cambios al archivo.
   ```

5. Copilot explorará el proyecto y aplicará los cambios, normalmente en el helper y sus pruebas.
6. Observa que el helper es una función tipada que recibe `db` como primer argumento y devuelve una matriz tipada de editoriales; así lo indican las convenciones de `.github/instructions/drizzle.instructions.md`, aplicables a `src/lib/*.ts`.
7. Observa que al código propuesto **le faltan** comentarios TSDoc y un encabezado de archivo.

> [!CAUTION]
> Copilot podría añadir comentarios aunque no se los pidas. No pasa nada; lo importante es mejorar la *consistencia* después de actualizar las instrucciones.

## Añadir un nuevo estándar al repositorio

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Localiza **Code formatting requirements**, cerca de la línea 27. Aún no incluye instrucciones para documentar el código.
3. Añade estas líneas de Markdown debajo de los estándares existentes:

   ```markdown
   - Todas las funciones exportadas deben incluir un comentario TSDoc que describa su propósito, sus parámetros y su valor de retorno.
   - Antes de las importaciones o de cualquier código, añade al archivo un bloque de comentarios que explique su propósito.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, puedes crear archivos de instrucciones en el repositorio (`.github/copilot-instructions.md`) para proporcionar directrices globales o usar archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretos. El archivo a nivel de repositorio es el lugar adecuado para estándares generales como la regla de comentarios de documentación que acabas de añadir.

## Volver a ejecutar el prompt y observar el cambio

1. En Copilot Chat, selecciona **New Chat**.
2. Vuelve a `src/lib/publishers.ts`.
3. Envía el mismo prompt:

   ```plaintext
   Crea o actualiza src/lib/publishers.ts con un helper de acceso a datos que devuelva la lista de todas las editoriales, con el nombre y el id de cada una. Aplica los cambios al archivo.
   ```

4. Observa que el archivo propuesto ahora empieza con un bloque similar a este:

   ```typescript
   /**
    * Helpers de acceso a datos de editoriales para la plataforma de financiación
    * colectiva de Tailspin Toys.
    * Proporciona funciones para recuperar información de editoriales de la base de datos.
    */
   ```

5. La función también debería incluir un comentario TSDoc como este:

   ```typescript
   /**
    * Devuelve una lista de todas las editoriales con su id y su nombre.
    *
    * @param db - El cliente de base de datos de Drizzle.
    * @returns Una promesa que se resuelve en una matriz de objetos de editoriales.
    */
   ```

Acabas de orientar a Copilot para que siga un estándar del proyecto y lo aplique a código real que ampliarás en el ejercicio siguiente. Mantén actualizado este archivo: será la primera parte de los datos de filtrado sobre la que trabajarás después.

## Confirmar las instrucciones y enviar la rama

Los archivos de instrucciones, como cualquier otro recurso, se gestionan mediante el mismo proceso de control de versiones que los demás archivos del proyecto.

1. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Desde el terminal, confirma que están presentes la actualización de instrucciones y los cambios del helper:

   ```bash
   git status
   ```

3. Prepara y confirma la actualización de instrucciones y todos los archivos que Copilot haya modificado para crear la base del helper:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra otros cambios relacionados (por ejemplo, pruebas), añade también esos archivos.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envía la rama:

   ```bash
   git push -u origin custom-instructions
   ```

## Crear y fusionar una pull request

Con la rama enviada, abre una pull request y relaciónala con la incidencia del backlog sobre los estándares de documentación. Pide a Copilot que encuentre la incidencia y cree una PR que la cierre e indique que también se ha añadido la base del helper de editoriales.

1. Abre Copilot Chat en tu codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y confirma que **Agent** está seleccionado.
3. Envía este prompt:

   ```
   Busca la incidencia relacionada con la actualización del archivo de instrucciones. Crea una pull request desde la rama actual, indica que cierra esa incidencia e incluye que también hemos añadido la base del helper de editoriales para el próximo trabajo de filtrado.
   ```

4. Copilot buscará la incidencia y creará la PR.
5. Cuando Copilot solicite permiso para realizar acciones de GitHub, revisa el comando y selecciona **Allow** cuando corresponda.
6. Cuando se cree la PR, pídele que la fusione y vuelva a `main`:

   ```
   Fusiona la PR en main. Después, vuelve localmente a main y obtén el código más reciente para tener el repositorio actualizado.
   ```

7. Cuando Copilot solicite permiso para realizar acciones de GitHub y ejecutar comandos de shell, revisa el comando y selecciona **Allow** cuando corresponda.

Ya has creado y fusionado una pull request con ayuda de GitHub Copilot.
## Resumen y siguientes pasos

Has explorado cómo Copilot obtiene contexto de los archivos de instrucciones de este proyecto. Después, usaste Copilot Chat en VS Code para:

- enviar un prompt de generación de código y observar el resultado con las instrucciones *existentes*;
- añadir un nuevo estándar general a `.github/copilot-instructions.md`;
- volver a enviar el mismo prompt y observar cómo el código propuesto sigue el estándar nuevo;
- confirmar en `main` la actualización de instrucciones y la base del helper para que el siguiente ejercicio pueda continuar el trabajo.

A continuación, aplicarás las instrucciones en [modo de agente][next-lesson] para añadir una funcionalidad.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Buenas prácticas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personales para GitHub Copilot][personal-instructions]
- [Awesome Copilot: archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
