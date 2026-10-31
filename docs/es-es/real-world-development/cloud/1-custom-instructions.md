---
title: "Lección 1: Instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Agente de Copilot en la nube →][next-lesson]

El contexto es fundamental al trabajar con IA generativa. Si una tarea debe realizarse de una forma concreta, o Copilot necesita información previa, debes proporcionarle ese contexto. Los [archivos de instrucciones][instruction-files] indican no solo qué quieres que haga Copilot, sino también cómo debe hacerlo.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto del proyecto, las directrices de programación y los estándares de documentación mediante instrucciones personalizadas del repositorio e instrucciones específicas de rutas.
- añadirás un estándar general al archivo `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los recorridos de VS Code y CLI, aquí no compararás prompts antes y después. El agente de Copilot en la nube trabaja de forma asíncrona con incidencias de GitHub, por lo que es más difícil mostrar el efecto en tiempo real. Verás cómo influyen las instrucciones más adelante, al revisar las solicitudes de incorporación de cambios que genere.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene directrices y requisitos para las prácticas de desarrollo:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz debe usar el modo oscuro y un diseño moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Cada archivo debe empezar con un bloque de comentarios que describa su contenido.

Usarás archivos de instrucciones para que Copilot trabaje de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias para que Copilot entienda el estilo de programación y los requisitos. Puedes especificar convenciones, bibliotecas y los tipos de comentarios que debe incluir el código. Hay dos tipos:

- `.github/copilot-instructions.md`: un único archivo que se envía a Copilot en todas las solicitudes del repositorio. Debe contener información general del proyecto, como la pila tecnológica, qué se está creando, las prácticas recomendadas y otras directrices.
- `.github/instructions/*.instructions.md`: archivos con directrices para determinados lenguajes, tipos de archivo o tareas, como crear un componente de interfaz o pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se usan para generar código en Copilot Chat, no para completar código ni sugerir ediciones. Copilot Chat, Copilot CLI y el agente en la nube usan tanto las instrucciones del repositorio como los archivos `*.instructions.md` con frontmatter `applyTo`. Copilot también admite otros formatos, como AGENTS.md y CLAUDE.md; consulta la [referencia de compatibilidad][custom-instructions-support].

### Prácticas recomendadas para gestionar archivos de instrucciones

El proyecto de ejemplo muestra un enfoque habitual:

- Mantén `copilot-instructions.md` centrado en directrices generales, como la descripción y estructura del proyecto y los estándares de programación.
- Usa `*.instructions.md` para instrucciones específicas de tipos de archivo —pruebas unitarias, componentes Astro o capa de datos— o de tareas.
- Usa lenguaje natural, redacta instrucciones claras e incluye ejemplos de cómo debería ser el código.

No hay una única forma correcta de crear archivos de instrucciones ni de usar IA; la experimentación te ayudará a encontrar la que mejor se adapte al proyecto.

> [!TIP]
> Los proyectos que usan GitHub Copilot deberían tener un conjunto sólido de archivos de instrucciones. En este proyecto encontrarás directrices para varias tareas, como [actualizar la interfaz][ui-instructions] y trabajar con [Astro][astro-instructions].
>
> Copilot también puede ayudar a crear archivos de instrucciones. Cada entorno ofrece esta opción de forma distinta; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. La lección correspondiente explica cuándo usarla.
>
> Para encontrar plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], un repositorio de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones del proyecto

Lee los archivos incluidos en el repositorio: hay un `copilot-instructions.md` principal y varios `*.instructions.md` para distintas tareas. Ábrelos en la interfaz web de GitHub o en un Codespace.

1. Abre `.github/copilot-instructions.md`.
2. Lee la descripción breve del proyecto y las secciones **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa también las directrices de **GitHub Actions Workflows**. Se aplican a todas las interacciones con Copilot.
3. Explora la carpeta `.github/instructions`. Hay instrucciones para archivos Astro, la capa de datos Drizzle, pruebas y otros aspectos.
4. Abre `.github/instructions/unit-tests.instructions.md` y observa el campo `applyTo`: define un patrón glob relativo a la raíz del repositorio para determinar a qué archivos se aplican las instrucciones. En este caso, coincide con archivos de pruebas TypeScript como `**/*.test.ts`.
5. Lee las instrucciones específicas para crear pruebas unitarias.
6. Abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones y a archivos existentes del proyecto. Así puedes dividir las directrices en archivos pequeños y reutilizables, y señalar a Copilot ejemplos que puede seguir. Las rutas son relativas al archivo de instrucciones.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` describe los estándares del proyecto, pero aún no requiere documentación en el código. Añadirás reglas para comentarios TSDoc y comentarios iniciales en los archivos.

## Añade un estándar para todo el repositorio

Añadirás una regla general para que la documentación se escriba en el código mediante comentarios TSDoc y un comentario inicial para cada archivo. El agente en la nube la utilizará cuando trabaje en incidencias que le asignes más adelante.

Antes de editar, crea una rama. El agente lee las instrucciones de la rama de destino de la incidencia, pero así podrás confirmar los cambios de forma ordenada.

1. En el terminal del Codespace, crea y selecciona una rama:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. Vuelve a abrir `.github/copilot-instructions.md`.
3. Localiza la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Aún no incluye instrucciones para documentar el código.
4. Añade estas líneas Markdown debajo de los estándares actuales:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

5. Guarda `copilot-instructions.md`.

> [!TIP]
> Las instrucciones generales del repositorio se guardan en `.github/copilot-instructions.md`; las instrucciones específicas de lenguajes, archivos o tareas van en `*.instructions.md`. El archivo general es el lugar adecuado para un estándar que se aplica a todo el proyecto, como el de comentarios que acabas de añadir.

## Confirma, publica y combina la actualización de instrucciones

El agente en la nube lee las instrucciones de la rama a la que apunta la incidencia. En el siguiente ejercicio, Copilot partirá de `main`; por eso, los cambios de instrucciones deben llegar a `main`.

1. Prepara y confirma el cambio:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Publica la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una solicitud de incorporación de cambios desde `custom-instructions-cloud` a `main` y combínala. Así, el agente en la nube leerá estas instrucciones cuando trabaje en las incidencias del siguiente ejercicio.

> [!TIP]
> Si prefieres trabajar directamente en `main` durante este taller, puedes omitir la rama y confirmar los cambios directamente en `main`. Se incluye la rama para reflejar cómo se haría en un proyecto real.

## Resumen y siguientes pasos

Has explorado cómo Copilot usa el contexto de los archivos de instrucciones y has añadido un estándar general a `.github/copilot-instructions.md`. Verás cómo influye en las solicitudes de incorporación de cambios que genere el agente en la nube.

A continuación, [asignarás tu primera incidencia al agente de Copilot en la nube][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para redactar mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: colección de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
