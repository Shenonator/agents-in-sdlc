---
title: "Lección 1: Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo agente →][next-lesson]

El contexto es fundamental al trabajar con IA generativa. Si una tarea debe realizarse de una forma concreta, o Copilot necesita información previa, debes proporcionarle ese contexto. Los [archivos de instrucciones][instruction-files] permiten orientar la generación de código.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto del proyecto, las directrices de programación y los estándares de documentación mediante instrucciones personalizadas del repositorio e instrucciones específicas de rutas.
- generarás la primera parte de los datos para el filtrado (una función auxiliar de editoriales) con las instrucciones actuales.
- añadirás un estándar general al archivo `.github/copilot-instructions.md`.
- volverás a ejecutar el mismo prompt y observarás cómo el código generado sigue el nuevo estándar.
- confirmarás los cambios de instrucciones y la función auxiliar de filtrado en `main` para que Copilot pueda usar las directrices actualizadas en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede no seguir todos los estándares que hayas definido. Copilot es no determinista. El objetivo es observar la tendencia del cambio de comportamiento tras actualizar las instrucciones, no obtener código idéntico carácter por carácter.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene directrices y requisitos para las prácticas de desarrollo:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz debe usar el modo oscuro y un diseño moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Cada archivo debe empezar con un bloque de comentarios que describa su contenido.

Usarás archivos de instrucciones para que Copilot disponga del contexto necesario y pueda trabajar de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias para que Copilot entienda mejor el estilo de programación y los requisitos. Puedes especificar convenciones, bibliotecas y los tipos de comentarios que debe incluir el código. Hay dos tipos de archivos:

- `.github/copilot-instructions.md`: un único archivo que se envía a Copilot en todas las solicitudes del repositorio. Debe contener información relevante para la mayoría de las interacciones, como la pila tecnológica, una descripción del proyecto, las prácticas recomendadas y directrices generales.
- `.github/instructions/*.instructions.md`: archivos con directrices para determinados lenguajes, tipos de archivo o tareas, como crear un componente de interfaz o pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se usan para generar código en Copilot Chat, no para completar código ni sugerir ediciones. Copilot Chat, Copilot CLI y el agente en la nube usan tanto las instrucciones del repositorio como los archivos `*.instructions.md` con frontmatter `applyTo`. Copilot también admite otros formatos, como AGENTS.md y CLAUDE.md; consulta la [referencia de compatibilidad][custom-instructions-support].

### Prácticas recomendadas para gestionar archivos de instrucciones

El proyecto de ejemplo muestra un enfoque habitual:

- Mantén `copilot-instructions.md` centrado en directrices generales: qué se está creando, cómo se estructura el proyecto y qué estándares de programación se aplican.
- Usa archivos `*.instructions.md` para indicar pautas específicas por tipo de archivo —pruebas unitarias, componentes Astro o capa de datos— o por tarea.
- Usa lenguaje natural, redacta instrucciones claras e incluye ejemplos de cómo debería ser el código.

No hay una única forma correcta de crear instrucciones o de usar IA; la experimentación te ayudará a encontrar la que mejor se adapte al proyecto.

> [!TIP]
> Los proyectos que usan GitHub Copilot deberían tener un conjunto sólido de archivos de instrucciones. En este proyecto encontrarás directrices para varias tareas, como [actualizar la interfaz][ui-instructions] y trabajar con [Astro][astro-instructions].
>
> Copilot también puede ayudar a crear archivos de instrucciones. Cada entorno ofrece esta opción de forma distinta; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. La lección correspondiente explica cuándo usarla.
>
> Para encontrar plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizados del proyecto

Lee los archivos de instrucciones incluidos en el repositorio: hay un `copilot-instructions.md` principal y varios archivos `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Lee la descripción del proyecto y las secciones **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa también las directrices de **GitHub Actions Workflows**. Se aplican a cualquier interacción con Copilot.
3. Explora la carpeta `.github/instructions`. Hay instrucciones para archivos Astro, la capa de datos Drizzle, pruebas y otros aspectos.
4. Abre `.github/instructions/unit-tests.instructions.md` y observa el campo `applyTo`: define un patrón glob relativo a la raíz del repositorio para determinar a qué archivos se aplican las instrucciones. En este caso, coincide con los archivos de pruebas TypeScript, como `**/*.test.ts`.
5. Lee las instrucciones específicas para crear pruebas unitarias.
6. Abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones —por ejemplo, `unit-tests.instructions.md`— y a archivos existentes del proyecto. Así puedes dividir conjuntos grandes de instrucciones en archivos pequeños y reutilizables, y señalar a Copilot ejemplos que puede seguir. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` describe los estándares de programación del proyecto, pero aún no requiere documentación en el código. En los siguientes pasos añadirás reglas para comentarios TSDoc y comentarios iniciales en los archivos.

## Crea una rama para los cambios

Crea una rama nueva para este trabajo.

1. Vuelve al Codespace del ejercicio anterior.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea y selecciona una rama:

   ```bash
   git checkout -b custom-instructions
   ```

## Usa Copilot Chat antes de actualizar las instrucciones

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar, vuelve al Codespace, abre Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; las instrucciones de cada paso indican cuándo debes seleccionarlos.

Para observar el efecto de las instrucciones personalizadas, envía primero un prompt con el archivo de instrucciones actual. Después lo actualizarás y enviarás el mismo prompt para comparar el resultado.

1. Cierra las pestañas abiertas de ejercicios anteriores para que Copilot solo use el contexto necesario.
2. Abre `src/lib/publishers.ts` para indicarle dónde debe estar la función auxiliar.
3. En el selector de agentes de Chat, selecciona **Agent** para que Copilot pueda aplicar cambios.

   ![Selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envía este prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explora el proyecto y aplica cambios, a menudo en el archivo auxiliar y en sus pruebas.
6. Observa que la función propuesta tiene tipos, acepta un cliente `db` como primer argumento y devuelve una matriz de editoriales con tipos. Esto se debe a las convenciones de la capa de datos de `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Observa que el código propuesto **no incluye** comentarios TSDoc ni un comentario inicial para el archivo.

> [!CAUTION]
> Como Copilot es probabilístico, puede que añada comentarios de documentación aunque no se lo hayas pedido. No pasa nada; el objetivo es observar la mejora de coherencia tras actualizar las instrucciones.

## Añade un estándar para todo el repositorio

El archivo `.github/copilot-instructions.md` proporciona información general del proyecto a Copilot. Añade un estándar para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Localiza la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Describe los estándares de programación, pero todavía no exige documentación en el código.
3. Añade estas líneas Markdown debajo de los estándares actuales para pedir comentarios iniciales y comentarios TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto, las instrucciones pueden aplicarse a todo el repositorio mediante `.github/copilot-instructions.md` o a lenguajes, tipos de archivo y tareas concretos mediante `*.instructions.md`. El archivo general es adecuado para estándares que se aplican a todo el proyecto, como la documentación del código.

## Vuelve a enviar el prompt y observa el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat** para limpiar el contexto.
2. Haz clic en `src/lib/publishers.ts` para que Copilot se centre en el archivo correcto.
3. Envía el mismo prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observa que el archivo propuesto ahora empieza con un bloque similar a este:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observa que la función ahora incluye un comentario TSDoc similar a este:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Has orientado a Copilot para que siga un estándar del proyecto y lo aplique a código real que utilizarás en el siguiente ejercicio.

## Confirma las instrucciones y publica la rama

Los archivos de instrucciones se gestionan con el mismo control de versiones que los demás archivos del proyecto. Confirma los cambios y publica la rama.

1. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Comprueba que están presentes las instrucciones actualizadas y los cambios de la función auxiliar:

   ```bash
   git status
   ```

3. Prepara y confirma las instrucciones y todos los archivos modificados por Copilot para la función auxiliar:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Publica la rama:

   ```bash
   git push -u origin custom-instructions
   ```

## Crea y combina una solicitud de incorporación de cambios

Con la rama publicada, crea una PR asociada a la incidencia del backlog sobre los estándares de documentación. Copilot puede buscar la incidencia, crear la PR y asociarla a ella.

> [!NOTE]
> Este es el primer ejercicio en el que Copilot actúa en GitHub. La plantilla del proyecto ya configura las herramientas de GitHub en el entorno; no tienes que instalar nada. La primera vez que Copilot use una herramienta, VS Code puede pedirte que inicies sesión. Sigue las instrucciones. En un ejercicio posterior aprenderás cómo funciona esta conexión (Model Context Protocol).

1. Abre Copilot Chat en el Codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y confirma que está seleccionado **Agent**.
3. Pide a Copilot que encuentre la incidencia de actualización de las instrucciones y cree una PR desde la rama actual. Indica que debe incluir la actualización de instrucciones y la base de la función auxiliar de editoriales:

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot buscará la incidencia y creará la PR.
5. Si Copilot pide permiso para realizar acciones de GitHub, revisa la solicitud y selecciona **Allow** si estás de acuerdo.
6. Cuando se cree la PR, pídele que la combine y vuelva a la rama principal local:

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Si Copilot pide permiso para realizar acciones de GitHub o ejecutar comandos de shell, revisa la solicitud y selecciona **Allow** si estás de acuerdo.

Has creado y combinado una solicitud de incorporación de cambios con GitHub Copilot.

## Resumen y siguientes pasos

Has explorado cómo Copilot obtiene el contexto de los archivos de instrucciones y has usado Copilot Chat en VS Code para:

- enviar un prompt de generación de código y observar el resultado con las instrucciones actuales.
- añadir un estándar general a `.github/copilot-instructions.md`.
- repetir el prompt y observar cómo el código propuesto adopta el estándar.
- confirmar las instrucciones y la base de la función auxiliar en `main` para que el siguiente ejercicio pueda aprovecharlas.

A continuación, aplicarás esas instrucciones en el [modo agente][next-lesson] para añadir una funcionalidad en varias partes del código.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para redactar mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personalizadas personales para GitHub Copilot][personal-instructions]
- [Awesome Copilot: colección de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
