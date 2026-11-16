---
title: "Ejercicio 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot son asistentes de IA especializados en tareas o ámbitos concretos del desarrollo. Sus archivos Markdown en `.github/agents` incluyen instrucciones, buenas prácticas, patrones de código y conocimientos del dominio. Los equipos pueden codificar su experiencia en agentes reutilizables, como uno de accesibilidad que aplique [WCAG][wcag], uno de seguridad o uno de pruebas.

Los agentes se definen en archivos Markdown del proyecto o, globalmente, en `~/.copilot/agents`. Cada archivo tiene frontmatter YAML con `name` y `description`, seguido de instrucciones que definen el comportamiento y la especialización.

### Agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] se definen principalmente mediante archivos Markdown. La diferencia más sencilla es que el **agente personalizado** es quien realiza el trabajo y las **habilidades** son herramientas. Un agente personalizado tiene su propia ventana de contexto y puede coordinar habilidades y otros agentes. En este laboratorio, el agente de accesibilidad revisará el sitio y podría invocar habilidades de flujo de pull request o de pruebas.

> [!NOTE]
> No hay una única forma correcta de crear agentes personalizados. Prueba e itera para descubrir qué funciona en tus entornos.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Escenario

Tailspin Toys quiere que su plataforma de financiación colectiva sea accesible para todas las personas. Algunos usuarios encuentran difícil leer el tema oscuro por el contraste insuficiente. El equipo de diseño ha solicitado un modo de alto contraste que se pueda activar o desactivar. Usarás un agente personalizado para implementar esta función.

En este ejercicio revisarás un agente personalizado de accesibilidad y lo usarás en Copilot Chat.

## Revisar el agente personalizado de accesibilidad

El proyecto ya incluye un agente de accesibilidad. Vuelve al codespace y revisa el archivo:

1. Abre `.github/agents/accessibility.md`.
2. Examina el frontmatter YAML y los campos `name` y `description`.

> [!CAUTION]
> El frontmatter con `name` y `description` es obligatorio para los agentes personalizados.

3. Revisa las responsabilidades, las buenas prácticas, los ejemplos de HTML, CSS y JavaScript, y los errores habituales.

## Usar el agente personalizado en Copilot Chat

VS Code muestra los agentes personalizados definidos en `.github/agents` en el selector situado en la parte inferior de Copilot Chat. Puedes seleccionar uno para aplicar sus instrucciones y herramientas a la sesión.

> [!TIP]
> **Abrir Copilot Chat**
>
> Vuelve al codespace, abre Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio.

1. Si hace falta, selecciona **Agent** en el selector de agentes.

   ![Selector de agente de la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Abre el selector de agentes en la parte inferior de Chat; de forma predeterminada muestra **default**.
3. Selecciona **Accessibility agent**.
4. Envía este prompt:

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot explorará el código, propondrá cambios y editará los archivos del proyecto. Cada edición aparecerá en el chat con la ruta del archivo y un diff.
6. El indicador **Files changed** mostrará los archivos modificados durante la sesión.

> [!NOTE]
> El proceso puede tardar unos minutos. Copilot realizará cambios reales en el repositorio, como modificar componentes Astro, CSS y pruebas.

Revisarás y orientarás el trabajo en el siguiente ejercicio.

## Resumen y siguientes pasos

Has explorado los [agentes personalizados][custom-agents], asistentes especializados que permiten reutilizar los conocimientos y estándares del equipo. Has aprendido cómo se definen y cómo usar uno en el modo de agente de Copilot Chat.

A continuación, [supervisarás y orientarás el trabajo del agente][next-lesson], revisarás los cambios a medida que se realicen y añadirás un control para el modo claro en la misma sesión.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Crear agentes personalizados en el IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados en awesome-copilot][awesome-copilot-agents]

---

| [← Lección anterior: Probar la funcionalidad con el servidor MCP de Playwright][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
