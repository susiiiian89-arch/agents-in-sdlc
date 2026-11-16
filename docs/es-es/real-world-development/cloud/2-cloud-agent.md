---
title: "Ejercicio 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que muy pocas organizaciones —si es que hay alguna— no tengan problemas de deuda técnica. Puede tratarse de problemas de seguridad sin resolver, código heredado que hay que actualizar o funcionalidades que llevan tiempo en el backlog por falta de tiempo para implementarlas. GitHub Copilot cloud agent está diseñado para encargarse de tareas como actualizar código y añadir funcionalidades de forma autónoma. Al terminar, crea una pull request (PR) de borrador para que la revise un desarrollador. Así se pueden delegar tareas tediosas, acelerar el desarrollo y dejar que los desarrolladores se centren en cuestiones de mayor alcance.

Con Copilot cloud agent explorarás:

- cómo personalizar el entorno para generar código;
- cómo asegurarte de que las operaciones se realicen de forma segura;
- por qué es importante delimitar bien las incidencias;
- cómo asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica que quiere resolver. Los contratistas que crearon la primera versión del sitio dejaron la documentación en mal estado: comprobarás que falta por completo. Como primer paso, el equipo quiere añadir comentarios de documentación TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar el descubrimiento de juegos. Quiere mostrar juegos relacionados —otros títulos de la misma categoría— en la página de detalles de cada juego para que los visitantes puedan seguir explorando. Todavía no necesita una implementación pulida; solo quiere algo que sirva para probar la experiencia. Esta tarea está bloqueada, pero hay otras incidencias de mayor prioridad.

Ambas son tareas que pueden quedar relegadas y que resultan apropiadas para asignar a Copilot cloud agent. El agente puede trabajar en ellas de forma asíncrona mientras el desarrollador se ocupa de otras tareas y, después, puede revisar el resultado.

## Presentación de GitHub Copilot cloud agent

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano de forma parecida a un desarrollador. Al igual que con una persona, hay varias formas de asignar el trabajo, entre ellas [asignar una incidencia de GitHub a Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Cuando se le asigna una incidencia, Copilot crea una PR de borrador para registrar el progreso, configura un entorno y empieza a trabajar. Puedes consultar la sesión mientras sigue activa o después de que termine. Cuando la solución esté lista para que la revises, te mencionará en la pull request.

## La importancia de unas instrucciones bien delimitadas

GitHub Copilot no es magia: no puedes saltarte el trabajo de pensar. Incluso las tareas aparentemente sencillas pueden ser complejas cuando se examinan de cerca, así que [delimita con cuidado las tareas que asignas a Copilot cloud agent][cloud-agent-best-practices]. Trátalo como a un compañero de programación con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. Los fundamentos del desarrollo de software no cambian por incorporar IA generativa.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores se han presentado las instrucciones personalizadas y cómo orientan a Copilot. Si ya completaste el ejercicio práctico de instrucciones personalizadas, habrás visto `.github/copilot-instructions.md` en acción; si no, este es un buen momento para revisarlo. Antes de asignar trabajo a Copilot cloud agent, inspecciona en modo de solo lectura los archivos de instrucciones del repositorio para poder reconocer su efecto más adelante.

Abre estos archivos en la interfaz web de GitHub o en un codespace:

- `.github/copilot-instructions.md`: revisa **Code standards**, especialmente las expectativas sobre las convenciones de TypeScript y los comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: fíjate en el frontmatter `applyTo`, que aplica estas instrucciones a los archivos `**/*.test.ts`.

Cuando en la siguiente sección asignes a cloud agent la incidencia `Code lacks documentation`, busca en la PR los comentarios TSDoc, las convenciones de TypeScript y los encabezados de comentario. Proceden de estos archivos de instrucciones. Volverás a revisarlos en un ejercicio posterior.

## Configurar el entorno de desarrollo de Copilot cloud agent

Crear código suele requerir un entorno específico y la ejecución de scripts de configuración para que todo esté listo, independientemente de quién haga el trabajo. Lo mismo ocurre al asignar tareas a Copilot, que trabaja de forma parecida a un ingeniero de software.

Cloud agent usa [GitHub Actions][github-actions] como entorno de trabajo. Puedes personalizarlo con un [flujo de configuración especial][setup-workflow], definido en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que el agente empiece. Así tendrá las herramientas de desarrollo y dependencias necesarias. Este flujo ya está configurado para el laboratorio: proporciona Node.js, las dependencias del proyecto, los navegadores para las pruebas end-to-end y la base de datos SQLite migrada y precargada de la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Concede los permisos mínimos necesarios para *tus pasos*. Copilot recibirá su propio token para sus operaciones.
    permissions:
      # Si quieres clonar el repositorio como parte de la configuración, por ejemplo, para instalar dependencias, necesitarás el permiso `contents: read`. Si no lo clonas aquí, Copilot lo hará automáticamente al terminar estos pasos.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración de frontend y aplicación: Node.js (la aplicación completa usa Astro y Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y precarga la base de datos SQLite local para que la compilación y las pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro flujo de GitHub, pero tiene algunos aspectos importantes:

- Contiene un único trabajo llamado `copilot-setup-steps`. Se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la pull request.
- El desencadenador `workflow_dispatch` permite ejecutar manualmente el flujo desde la pestaña **Actions** del repositorio. Así puedes comprobar que funciona sin esperar a que Copilot lo ejecute.

## Añadir documentación

Aunque todo el mundo reconoce la importancia de la documentación, muchos proyectos tienen información obsoleta o carecen de ella. Esta deuda técnica suele quedar sin resolver, reduce la productividad y dificulta mantener el código o incorporar a nuevos desarrolladores. Copilot destaca en la creación de documentación, por lo que esta incidencia es idónea para asignársela a cloud agent. El agente generará la documentación en segundo plano y revisarás su trabajo en un ejercicio posterior.

1. Abre tu repositorio en github.com en una pestaña nueva del navegador.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el diálogo de incidencias.
4. Selecciona **Blank issue** para crear una incidencia en blanco.
5. Escribe `Code lacks documentation` en el campo **Title**.
6. Escribe lo siguiente en **Description**:

    ```plaintext
    Nuestra organización exige que las funciones y los métodos incluyan comentarios de documentación TSDoc cuando sean útiles. Por desgracia, las actualizaciones recientes no han seguido este estándar. Debemos actualizar el código existente para asegurarnos de que se incluyan comentarios de documentación cuando aclaren el comportamiento.
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el diálogo de asignación.

  ![Asignación de una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

  ![Detalles de la asignación a Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecciona la pestaña **Pull Requests**.
11. Abre la nueva pull request (PR), cuyo título será parecido a `[WIP]: Code lacks documentation`. Si aún no aparece en la lista, espera un momento y actualiza la ventana del navegador.
12. Al cabo de unos minutos, Copilot debería haber creado una lista de tareas.

> [!NOTE]
> La lista puede tardar varios minutos en aparecer en la PR. Copilot está preparando su entorno mediante el flujo descrito antes, analizando el proyecto y determinando cómo abordar el problema.

13. Revisa la lista y las tareas que Copilot va a realizar.
14. Desplázate hacia abajo por la cronología de la pull request. Deberías ver un aviso de que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona **View session**.

  ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la ventana para ver el indicador actualizado.

16. Recorre la sesión en directo para ver cómo Copilot resuelve el problema: explora el código y comprende el estado del proyecto, se detiene a pensar y decidir el plan adecuado, y crea código.

El proceso puede tardar varios minutos. Uno de los objetivos principales de Copilot cloud agent es realizar tareas de forma asíncrona para que puedas centrarte en otras cosas. Aprovecharás esta función asignándole otra tarea y, mientras tanto, escribirás código para añadir funcionalidad a la aplicación.

## Añadir una sección de juegos relacionados a la página de detalles

Una de las ventajas de GitHub Copilot cloud agent es que permite dividir el trabajo: tú puedes centrarte en una tarea mientras el agente se ocupa de otra. Aunque añadir una sección de juegos relacionados para el equipo de diseño no lleve mucho tiempo, puedes dedicar ese tiempo a otras tareas. Asignemos el trabajo a Copilot cloud agent.

1. Vuelve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el diálogo de incidencias.
4. Selecciona **Blank issue** para usar la plantilla en blanco.
5. Escribe `Show related games on the game details page` en **Title**.
6. Escribe lo siguiente en **Description**:

    ```markdown
    Queremos ayudar a los usuarios a descubrir más juegos mostrando juegos relacionados en la página de detalles de cada juego. El equipo de diseño quiere explorar la experiencia y hacer pruebas de aceptación. Estos son los requisitos:

   - Añadir un helper de acceso a datos en `src/lib/` que devuelva otros juegos de la misma categoría que un juego, sin incluir ese juego.
   - Mostrar en la página de detalles una sección "Related games" que use el helper.
   - Gestionar el caso en que un juego no tenga juegos relacionados.
   - Crear pruebas unitarias para el helper nuevo.
   - Asegurarse de que todas las pruebas pasan antes de crear la PR.
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el diálogo de asignación.

  ![Asignación de una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Poco después, deberías ver varios 👀 en el primer comentario de la incidencia. Indican que Copilot ya está trabajando.

![Copilot usa el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en la segunda solicitud. Cloud agent trabaja de forma parecida a un ingeniero de software, así que no hace falta supervisarlo continuamente; revisa el resultado cuando termine. Ahora crearás y usarás agentes personalizados.

## Resumen y siguientes pasos

En esta lección has explorado [GitHub Copilot cloud agent][copilot-agents]. Puedes asignarle incidencias para que trabaje de forma asíncrona, por ejemplo, para resolver deuda técnica, crear funcionalidades o ayudar a migrar código entre frameworks.

Has explorado cómo personalizar el entorno para generar código, realizar operaciones de forma segura, delimitar bien las incidencias y asignárselas a Copilot.

Mientras cloud agent trabaja en segundo plano, puedes [crear y usar agentes personalizados][next-lesson]. [Copilot cloud agent también puede usar servidores MCP][cloud-agent-mcp] y dispone de instrucciones personalizadas, que se han explorado en módulos anteriores.

## Recursos

- [Acerca de Copilot cloud agent][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Buenas prácticas para el flujo de configuración de Copilot cloud agent][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
