---
title: "Ejercicio 1 - Instrucciones personalizadas (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Copilot cloud agent →][next-lesson]

El contexto es clave cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta o Copilot necesita información adicional, asegúrate de que pueda acceder a ese contexto. Los [archivos de instrucciones][instruction-files] proporcionan directrices para que Copilot entienda no solo *qué* debe hacer, sino también *cómo*.

En este ejercicio explorarás cómo las instrucciones personalizadas del repositorio y los archivos con ámbito de ruta proporcionan a Copilot contexto del proyecto, pautas de código y estándares de documentación. Después añadirás un estándar general al repositorio.

> [!NOTE]
> A diferencia de los itinerarios de VS Code y CLI, aquí no compararás prompts antes y después. Copilot cloud agent trabaja de forma asíncrona con incidencias de GitHub, por lo que es más difícil demostrar el efecto en tiempo real. Verás la influencia del archivo de instrucciones cuando revises las pull requests que genere.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene directrices y requisitos para el desarrollo:

- La capa de datos siempre requiere pruebas unitarias.
- La interfaz debe usar el modo oscuro y tener un aspecto moderno.
- El código debe incluir comentarios de documentación TSDoc.
- Cada archivo debe empezar con un bloque de comentarios que describa su contenido.

Los archivos de instrucciones permiten proporcionar a Copilot el contexto necesario para seguir estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias para que Copilot entienda el estilo de desarrollo y los requisitos. Puedes especificar convenciones, bibliotecas y los comentarios que quieres incluir en el código. Puedes crear instrucciones para todo el repositorio o para tipos de archivo concretos.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`: un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información de ámbito general, como la pila tecnológica, una descripción del proyecto, buenas prácticas y otras directrices globales.
- `.github/instructions/*.instructions.md`: archivos para tipos de archivo o tareas concretos, como TypeScript, Astro, componentes de interfaz o pruebas unitarias.

> [!NOTE]
> En un IDE, los archivos de instrucciones solo se usan para generar código en Copilot Chat, no para las finalizaciones de código ni las sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y Copilot cloud agent usan las instrucciones del repositorio y los archivos `*.instructions.md` con frontmatter `applyTo` al generar código.
>
> Copilot también [admite otros formatos de instrucciones][custom-instructions-support], como AGENTS.md y CLAUDE.md.

### Buenas prácticas para gestionar archivos de instrucciones

Los ejemplos del proyecto muestran un enfoque posible:

- Mantén `copilot-instructions.md` centrado en directrices del proyecto: descripción, estructura y estándares globales.
- Usa `*.instructions.md` para tipos de archivo (pruebas unitarias, componentes Astro, capa de datos) o tareas concretas.
- Escribe en lenguaje natural, con directrices claras y ejemplos de código correctos e incorrectos.

No existe una sola forma correcta de crear archivos de instrucciones; experimenta para descubrir qué funciona en tu proyecto.

> [!TIP]
> Los proyectos con GitHub Copilot deberían contar con una colección sólida de archivos de instrucciones. En este proyecto encontrarás ejemplos para [actualizaciones de interfaz][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede generar archivos de instrucciones. Cada entorno ofrece una experiencia diferente (por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI); el itinerario correspondiente lo indicará.
>
> Para encontrar plantillas, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar los archivos de instrucciones personalizadas del proyecto

Lee los archivos de instrucciones del repositorio en la interfaz web de GitHub o en un codespace:

1. Abre `.github/copilot-instructions.md`.
2. Revisa la descripción y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, fíjate en las pautas anidadas de **GitHub Actions Workflows**.
3. Explora `.github/instructions`, donde encontrarás instrucciones para Astro, la capa de datos de Drizzle, pruebas y otras tareas.
4. Abre `.github/instructions/unit-tests.instructions.md` y revisa el campo `applyTo`, que define el glob (relativo a la raíz del repositorio) que determina los archivos aplicables; por ejemplo, `**/*.test.ts`.
5. Revisa las instrucciones específicas para las pruebas unitarias.
6. Abre `.github/instructions/drizzle.instructions.md` y examina los enlaces a otros archivos de instrucciones y ejemplos del proyecto. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` describe los estándares de código, pero todavía no exige documentación dentro del código. Añadirás reglas para comentarios TSDoc y encabezados de archivo.

## Añadir un nuevo estándar al repositorio

Este es el único cambio que harás en este ejercicio: añadir una regla general para que el agente de la nube documente el código con comentarios TSDoc y un encabezado de archivo. El agente usará la regla en las incidencias que le asignes más adelante.

Antes de editar, crea una rama:

```bash
git checkout -b custom-instructions-cloud
```

1. Abre `.github/copilot-instructions.md`.
2. Localiza **Code formatting requirements**, cerca de la línea 27. Observa que aún no contiene reglas de documentación dentro del código.
3. Añade estas líneas de Markdown debajo de los estándares existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda el archivo.

> [!TIP]
> Las instrucciones de ámbito general pertenecen a `.github/copilot-instructions.md`; las instrucciones para lenguajes, tipos de archivo o tareas concretas pueden ir en archivos `*.instructions.md`.

## Confirmar, enviar y fusionar la actualización de instrucciones

Cloud agent lee las instrucciones de la rama a la que apunta la incidencia. En el siguiente ejercicio, partirá de `main`, así que los cambios deben llegar a `main`.

1. Prepara y confirma el cambio:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envía la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una pull request desde `custom-instructions-cloud` a `main` y fusiónala. Cloud agent leerá estas instrucciones cuando trabaje en incidencias asignadas en el siguiente ejercicio.

> [!TIP]
> Para este taller también puedes trabajar directamente en `main` y omitir la rama. Se incluye el paso de la rama para reflejar cómo se suele trabajar en un proyecto real.

## Resumen y siguientes pasos

Has explorado cómo Copilot utiliza los archivos de instrucciones y has añadido un estándar general a `.github/copilot-instructions.md`. Verás cómo se aplica en las pull requests que genere cloud agent en el resto del itinerario.

A continuación, [asignarás tu primera incidencia a Copilot cloud agent][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Buenas prácticas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
