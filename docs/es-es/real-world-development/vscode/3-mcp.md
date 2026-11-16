---
title: "Ejercicio 3 - Probar tu funcionalidad con el servidor MCP de Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Modo de agente][previous-lesson] · [Siguiente lección: Agentes personalizados →][next-lesson]

Acabas de crear la funcionalidad de filtrado con el modo de agente. Antes de abrir una pull request, debes comprobar que funciona en el navegador. En lugar de recorrer la aplicación manualmente, conectarás el **servidor MCP de Playwright** y dejarás que Copilot controle un navegador real para probarla. Después, publicarás la rama y abrirás la PR.

En este ejercicio aprenderás a:

- entender qué es Model Context Protocol (MCP) y cómo los servidores MCP amplían Copilot con nuevas herramientas;
- añadir el servidor MCP de Playwright al espacio de trabajo;
- pedir a Copilot que lo use para probar manualmente la funcionalidad en un navegador;
- publicar la rama y abrir una pull request para el trabajo de filtrado.

## ¿Qué es Model Context Protocol (MCP)?

El modo de agente es mucho más potente cuando puede acceder a recursos externos al editor. Copilot lo consigue mediante Model Context Protocol (MCP), un estándar que permite al agente comunicarse con herramientas y servicios externos.

![Diagrama del funcionamiento interno del modo de agente y su interacción con el contexto, el LLM y las herramientas, incluidas las herramientas aportadas por servidores MCP y extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que los agentes de IA se comuniquen con herramientas y servicios externos en tiempo real. Así pueden acceder a información actualizada mediante recursos y realizar acciones en tu nombre mediante herramientas.

Se accede a estas herramientas y recursos a través de un servidor MCP, que actúa como puente entre el agente de IA y los servicios y herramientas externos. El servidor MCP gestiona la comunicación entre el agente y las herramientas externas, como las API existentes o herramientas locales como los paquetes de NPM. Cada servidor MCP ofrece al agente un conjunto distinto de herramientas y recursos.

Algunos servidores MCP populares:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server):** ofrece acceso a un conjunto de API para gestionar repositorios de GitHub. El agente puede, por ejemplo, crear repositorios, actualizar los existentes y gestionar incidencias y pull requests.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp):** permite automatizar navegadores con Playwright. El agente puede, por ejemplo, navegar por páginas web, rellenar formularios y hacer clic en botones.

Hay muchos otros servidores MCP que ofrecen acceso a distintas herramientas y recursos. GitHub aloja un [registro de MCP](https://github.com/mcp) para facilitar su descubrimiento y fomentar las contribuciones al ecosistema.

> [!CAUTION]
> En materia de seguridad, trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usar uno, revisa detenidamente su código fuente, verifica quién lo publica y evalúa las implicaciones de seguridad. Usa únicamente servidores MCP de confianza y ten cuidado al conceder acceso a recursos u operaciones sensibles.

## Revisar la configuración de MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en este espacio de trabajo de VS Code.

1. Abre `.vscode/mcp.json` en tu Codespace.
2. Deberías ver que ya está configurado un servidor `github`:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

La entrada `github` forma parte de la plantilla del proyecto; por eso Copilot pudo leer el backlog de incidencias en el ejercicio anterior. Utiliza el [servidor MCP remoto de GitHub][remote-github-mcp-server], así que no hay que instalar nada localmente: VS Code se conecta por HTTP y, la primera vez que Copilot usa una de sus herramientas, te autenticas con GitHub.

## Añadir el servidor MCP de Playwright

Ahora añadirás un segundo servidor. El [servidor MCP de Playwright][playwright-mcp-server] proporciona a Copilot un navegador que puede controlar, justo lo que necesitas para probar la funcionalidad.

1. En `.vscode/mcp.json`, añade una entrada `playwright` junto a `github` para que el archivo quede así:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Guarda el archivo.

A diferencia del servidor remoto `github`, `playwright` es un servidor **local**: VS Code lo inicia en tu equipo ejecutando `npx @playwright/mcp@latest`. La opción `--headless` indica a Playwright que ejecute el navegador sin mostrar una ventana, algo necesario en un Codespace, que no dispone de un escritorio donde mostrarla.

> [!NOTE]
> El proyecto Tailspin Toys ya usa Playwright para las pruebas end-to-end, así que normalmente el navegador necesario ya está instalado. Si Copilot indica más adelante que falta un navegador, pídele que ejecute `npx playwright install chromium` y vuelva a intentarlo.

## Iniciar el servidor y confiar en él

VS Code inicia los servidores MCP **cuando se necesitan**: la primera vez que Copilot requiere las herramientas de un servidor, VS Code lo inicia y te pide confirmar que confías en él.

1. En `.vscode/mcp.json`, aparece la acción **Start** encima de la entrada `playwright`. Selecciónala para iniciar el servidor y confirmar que la configuración es correcta.
2. Si VS Code te pregunta si confías en el servidor, revisa la configuración y selecciona la opción para confiar en él y permitir que se inicie.

Iniciarlo manualmente es opcional. Si omites este paso, VS Code iniciará el servidor y te pedirá que confíes en él la primera vez que Copilot necesite sus herramientas.

## Probar la funcionalidad de filtrado

El [servidor MCP de Playwright][playwright-mcp-server] proporciona a Copilot un navegador real que puede controlar. En lugar de recorrer la aplicación para comprobar el resultado, el agente puede abrir una página, navegar, aplicar filtros, leer los resultados y resumir lo que observa. Es la forma más rápida de confirmar desde la conversación que una funcionalidad se comporta como esperas.

Internamente, el servidor MCP de Playwright trabaja con el [árbol de accesibilidad][playwright-mcp-server] de la página, no con capturas de pantalla. Así, el agente razona sobre elementos estructurados y etiquetados —como botones, enlaces y elementos de lista— del mismo modo que lo hace la tecnología de asistencia. Por eso, una comprobación funcional rápida también sirve como una revisión básica de accesibilidad.

Con el servidor conectado y la aplicación en marcha, pide a Copilot que pruebe la funcionalidad que acabas de crear:

```text
Usa el servidor MCP de Playwright para abrir un navegador en la aplicación que está en marcha en http://localhost:4321 y comprobar la nueva funcionalidad de filtrado de juegos:

1. Ve a la página de juegos y anota cuántos aparecen.
2. Aplica un filtro de categoría y confirma que la lista solo muestra los juegos de esa categoría.
3. Quita el filtro y aplica uno de editorial; confirma que la lista solo muestra los juegos de esa editorial.
4. Combina un filtro de categoría y otro de editorial y confirma que los resultados respetan ambos.

Describe lo que observas en cada paso e indica cualquier comportamiento inesperado.
```

Copilot abrirá un navegador mediante el servidor MCP de Playwright, seguirá los pasos y te comunicará sus resultados. Compara el resumen con los criterios de aceptación de la incidencia. Si algo no parece correcto, haz preguntas de seguimiento o pídele que corrija el código antes de abrir una pull request.

> [!NOTE]
> La aplicación debe estar en marcha en `http://localhost:4321` para realizar esta prueba. Si has detenido el servidor de desarrollo, vuelve a iniciarlo antes de enviar la solicitud. La primera vez que Copilot use el servidor MCP de Playwright, quizá tenga que descargar un navegador; si indica que falta uno, pídele que ejecute `npx playwright install chromium` y lo vuelva a intentar.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publicar la rama y crear una pull request

Ahora que has comprobado que la funcionalidad funciona, puedes abrir una pull request (PR) para que el equipo la revise. El primer paso es publicar la rama `filtering-vscode`.

1. Abre el panel **Source Control** en el Codespace y revisa los cambios de Copilot.
2. Prepara los cambios seleccionando el icono **+**.
3. Genera un mensaje de commit con el botón **Sparkle**.

   ![Captura de pantalla del panel Source Control con los cambios realizados](../../../_images/ex3-source-control-changes.png)

4. Selecciona **Publish** para enviar la rama al repositorio.

## Crear la pull request

Hay varias formas de crear una pull request, como github.com y la interfaz de línea de comandos (CLI) de GitHub. Como ya estás trabajando con GitHub Copilot, deja que la cree por ti. Puede localizar la incidencia pertinente y asociarla a la PR.

1. Abre el panel Copilot Chat y selecciona **New Chat** para iniciar una sesión nueva.
2. Comprueba que **Agent** está seleccionado en el selector de agentes para que Copilot pueda usar las herramientas de GitHub.
3. Pide a Copilot que cree una PR:

   ```plaintext
   Busca en el repositorio la incidencia relacionada con el filtrado por categoría y editorial. Crea una pull request para la rama actual y asóciala a la incidencia correcta.
   ```

4. Cuando sea necesario, selecciona **Continue** para permitir que Copilot recopile información y realice las operaciones. La primera vez que use una herramienta de GitHub, puede que tengas que autenticarte; sigue las indicaciones para hacerlo.
5. Observa cómo Copilot busca entre las incidencias, encuentra la correcta y crea la PR.
6. Selecciona el enlace que genera Copilot para revisar la pull request, pero **no la fusiones todavía**.

## Resumen y siguientes pasos

¡Enhorabuena! En este ejercicio has:

- aprendido qué es Model Context Protocol (MCP) y cómo los servidores MCP amplían Copilot con nuevas herramientas;
- añadido el servidor MCP de Playwright al espacio de trabajo;
- usado el servidor para probar manualmente la funcionalidad de filtrado en un navegador antes de publicarla;
- publicado la rama y abierto una pull request para el trabajo de filtrado.

Has usado un servidor MCP para *probar* una funcionalidad, pero MCP es solo una de las formas de ampliar Copilot. A continuación, [crea un agente personalizado][next-lesson] para agilizar tareas concretas, como las revisiones de accesibilidad.

## Recursos

- [Qué es MCP y por qué habla todo el mundo de ello][mcp-blog-post]
- [Servidor MCP de Microsoft Playwright][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [Registro de GitHub MCP][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
