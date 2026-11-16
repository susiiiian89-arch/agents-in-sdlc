---
title: "Lección 6 - Validar la funcionalidad con Playwright MCP"
description: "Inspecciona o configura Playwright MCP en Copilot CLI y úsalo para explorar en el navegador la funcionalidad de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como ya hemos señalado, programar no consiste solo en escribir código. También hay que trabajar con datos y servicios externos, y permitir que Copilot use automatizaciones adicionales. Para ello sirven los servidores MCP: permiten a Copilot ir más allá de las funciones integradas en la CLI y acceder a más herramientas y servicios.

En esta lección:

- aprenderás qué es Model Context Protocol (MCP) y cómo lo usa Copilot CLI;
- añadirás el servidor Playwright MCP si aún no está disponible;
- pedirás al agente que controle un navegador y explore la funcionalidad de filtrado.

## Escenario

Las pruebas unitarias y end-to-end son importantes, pero para validar cambios en la interfaz hace falta interactuar con ella. Quieres que Copilot pueda usar el sitio web como lo haría una persona y automatizar la comprobación de los cambios para tener más confianza en que funcionan según lo previsto.

## ¿Qué es Model Context Protocol (MCP)?

[Model Context Protocol (MCP)][mcp-blog-post] ofrece a los agentes de IA una forma de comunicarse con herramientas y servicios externos. MCP les permite comunicarse con ellos en tiempo real, acceder a información actualizada y realizar acciones en tu nombre.

Se accede a las herramientas y los recursos mediante un servidor MCP, que actúa de puente entre el agente de IA y los servicios externos. Cada servidor representa un conjunto diferente de herramientas y recursos a los que el agente puede acceder.

Algunos servidores MCP conocidos son:

- **[GitHub MCP Server][github-mcp]**: proporciona acceso a API para gestionar repositorios de GitHub, incidencias y pull requests.
- **[Playwright MCP Server][playwright-mcp-server]**: proporciona capacidades de automatización del navegador mediante Playwright.

Hay muchos otros servidores MCP disponibles. GitHub aloja un [registro MCP][mcp-registry] que facilita descubrirlos y contribuir al ecosistema.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usar uno, revisa su código fuente, verifica quién lo publica y valora las implicaciones de seguridad.

## Añadir el servidor MCP de Playwright

Si Playwright MCP todavía no está disponible, añádelo para que Copilot pueda interactuar con el sitio como lo haría una persona.

> [!NOTE]
> Si Playwright MCP ya está configurado en Copilot CLI, omite estos pasos y continúa con la sección siguiente.

1. Vuelve a tu Codespace.
2. Abre el diálogo para añadir un servidor MCP escribiendo este comando en Copilot CLI:

   ```plaintext
   /mcp add
   ```

3. En el campo del nombre, escribe `playwright` y pulsa <kbd>Tab</kbd>.
4. Confirma el tipo de servidor **STDIO** pulsando <kbd>Enter</kbd> y, después, pulsa <kbd>Tab</kbd>.
5. Pega lo siguiente en el diálogo **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pulsa <kbd>Control</kbd>+<kbd>S</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para guardar el servidor MCP.
7. Pulsa <kbd>Esc</kbd> para salir del diálogo MCP.

## Pedir a Copilot que explore la funcionalidad con Playwright

Ya has comprobado manualmente que la funcionalidad se comporta como esperas. Ahora pídele a Copilot que haga lo mismo con el servidor Playwright MCP.

1. Envía este prompt para pedir a Copilot que valide la funcionalidad con Playwright MCP:

   ```plaintext
   Inicia la aplicación y usa Playwright MCP para comprobar el filtrado según la incidencia y el plan. Dime qué funciona y qué no, sin hacer cambios. Cuando termines, detén el servidor que hayas iniciado.
   ```

> [!NOTE]
> Normalmente no hace falta indicar a Copilot que use el servidor MCP: suele elegirlo por su cuenta. Aun así, señalarle la herramienta puede ofrecer resultados más uniformes y ahorrar algunos tokens.

2. Observa los pasos que Copilot realiza en el navegador para comprobar que la funcionalidad funciona.
3. Lee el informe y comprueba que todo se comporta según lo previsto.

Copilot iniciará el servidor, usará Playwright para interactuar con el sitio, detendrá el servidor y te entregará un informe.

## Resumen y siguientes pasos

¡Enhorabuena! Has usado el servidor Playwright MCP para explorar la funcionalidad en un navegador real desde Copilot CLI. En resumen:

- has aprendido qué es Model Context Protocol (MCP) y cómo lo usa Copilot CLI;
- has añadido el servidor Playwright MCP si aún no estaba disponible;
- has pedido al agente que controle un navegador y explore la funcionalidad de filtrado.

A continuación, [crearás un agente personalizado de QA][next-lesson] que combine la habilidad y las herramientas del navegador en un rol especializado.

## Recursos

- [¿Qué demonios es MCP y por qué todo el mundo habla de ello?][mcp-blog-post]
- [Servidor Playwright MCP de Microsoft][playwright-mcp-server]
- [Añadir servidores MCP a Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
