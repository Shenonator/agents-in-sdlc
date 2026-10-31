---
title: "Lección 2: Añade funcionalidad con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso los cambios sencillos suelen afectar a varios archivos y requieren tareas como ejecutar pruebas. El flujo habitual de desarrollo puede implicar localizar los archivos, editarlos, probar los cambios, depurar y volver a empezar si falta algo. El modo agente de Copilot puede explorar la estructura del proyecto, realizar cambios coordinados, ejecutar tareas como las pruebas y corregir los errores que detecte.

> [!NOTE]
> Aunque sus nombres se parecen, el modo agente y el agente en la nube ofrecen experiencias distintas. El modo agente trabaja en el IDE, con ciclos rápidos de comentarios e interacción. El agente en la nube trabaja de forma asíncrona, como un miembro del equipo, y se comunica mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás cómo el modo agente:

- explora el proyecto, identifica los archivos relevantes y coordina los cambios.
- implementa funcionalidades en la interfaz y la capa de datos.
- permite revisar los cambios y las pruebas antes de combinarlos.

## Escenario

A medida que crece la lista de juegos, quieres permitir que los usuarios filtren por categoría y editorial. En el ejercicio anterior ya añadiste una función auxiliar de editoriales; ahora completarás con el modo agente el resto del trabajo de datos, interfaz y pruebas.

## Ejecuta el sitio de Tailspin Toys

Antes de hacer cambios, explora el sitio para conocer sus funciones actuales.

Tailspin Toys es una plataforma de financiación colectiva para juegos de mesa con una temática de desarrollo. Permite mostrar juegos y sus detalles. El sitio es una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle ORM; no hay una API de backend independiente ni un framework de interfaz del lado cliente. Las funciones reutilizables de acceso a datos están en `src/lib/`, y la interactividad se implementa con pequeños elementos `<script>` de Astro y las API DOM estándar.

### Inicia el sitio

La tarea `npm run dev` inicia el único servidor de desarrollo de Astro. Para ejecutarlo en GitHub Codespaces:

1. Vuelve al Codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea y selecciona una rama para el filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio:

   ```bash
   npm run dev
   ```

   Cuando se inicie el servidor Astro, verás un mensaje similar a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir `http://localhost:4321` en un navegador, ciérralo seleccionando **x**.

5. Abre el sitio manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando `http://localhost:4321` en el terminal.

> [!NOTE]
> En Codespaces, al seleccionar el enlace localhost en el terminal, se redirige a `https://<your-codespace-name>-4321.app.github.dev/`, un túnel privado a tu Codespace, que ahora aloja el servidor web.

### Explora el sitio

Cuando el sitio esté en ejecución, explora sus funciones principales:

- **Página de inicio**: muestra juegos de mesa con títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, la editorial y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar, vuelve al Codespace, abre Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; las instrucciones de cada paso indican cuándo debes seleccionarlos.

La implementación inicial funciona, pero queremos ampliarla. Pide a GitHub Copilot que muestre el backlog de incidencias que se creó automáticamente al crear el proyecto a partir de la plantilla.

1. En el selector de agentes de Chat, selecciona **Agent**. El agente planifica e implementa cambios en varios archivos, ejecuta comandos de terminal y usa herramientas.

   ![Selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Este modelo se incluye porque se utilizó al crear el taller, no porque los autores lo recomienden por encima de otros. Se espera así ofrecer sugerencias relativamente coherentes, pero las respuestas pueden variar porque los modelos de lenguaje son probabilísticos. Es normal.

> [!NOTE]
> Debido al carácter probabilístico de los modelos de lenguaje, Copilot puede usar un comando MCP diferente, pero debería completar la tarea.

3. Pide a Copilot que muestre y priorice el backlog:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para permitir que enumere las incidencias.
5. Revisa la lista y la priorización que ha generado.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporciona contexto. Usarás las instrucciones de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explican cómo añadir funcionalidad al sitio.

1. En el Codespace, abre `.github/instructions/ui.instructions.md`.
2. Lee las directrices, que incluyen:
   - una descripción general de la arquitectura.
   - principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - enlaces a instrucciones específicas de tipos de archivo, como Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden enlazar a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas al archivo de instrucciones. Esto facilita reutilizar las directrices, dividirlas en partes manejables y ofrecer ejemplos y plantillas a Copilot.

## Implementa el filtrado

Para completar el filtrado, hay que hacer al menos tres cambios:

- añadir o ajustar la lógica restante en la capa de datos (`src/lib/`).
- añadir o actualizar las pruebas del comportamiento del filtrado.
- actualizar la página de listado de juegos con la interfaz de filtrado.

Las pruebas deben ejecutarse y superarse antes de combinar los cambios. El modo agente de Copilot puede encargarse de estas tareas.

1. Continúa la conversación actual con Copilot o selecciona **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Ejemplo de selección del archivo de instrucciones de interfaz.](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado.

   ![Selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirma que sigue seleccionado el modelo **Claude Sonnet 4.5**.
5. Pide a Copilot que implemente la funcionalidad según la incidencia del backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto para localizar los archivos de la capa de datos, la interfaz y las pruebas; después, modifica los archivos y ejecuta las pruebas.

   ![Copilot explora los archivos del proyecto.](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot puede tardar varios minutos en explorar, editar y ejecutar pruebas. Durante el proceso, el editor puede mostrar las opciones **Keep** y **Undo**. Cuando termine, aparecerá una opción para aceptar o deshacer cada cambio; no hace falta seleccionarlas mientras el trabajo está en curso.

7. Cuando Copilot lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Cuadro de diálogo de Copilot Chat que pide confirmación para ejecutar pruebas.](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede haber pausas y algunas pruebas pueden fallar mientras Copilot itera entre la generación de código y las pruebas. Esto es normal.

   ![Sesión completa de Copilot Chat en modo agente.](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Revisa el código generado para detectar posibles problemas.

> [!CAUTION]
> Revisa siempre el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador donde se ejecuta el sitio y explora la nueva funcionalidad.
11. Cuando hayas confirmado que funciona y revisado el código, selecciona **Keep** en Copilot Chat.

## Resumen y siguientes pasos

Has usado el modo agente de GitHub Copilot para añadir funciones al sitio Tailspin Toys. Has aprendido cómo puede:

- implementar funcionalidades en la interfaz y la capa de datos.
- explorar el proyecto, identificar los archivos relevantes y coordinar cambios.
- permitirte revisar los cambios y las pruebas antes de combinarlos.

A continuación, [probarás la funcionalidad con el servidor Playwright MCP][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio de exploración adicional: implementar paginación

A medida que crezca la lista de juegos, será necesario añadir paginación. Usa lo aprendido para pedir a Copilot que la implemente. Sigue las instrucciones existentes, decide si el usuario podrá elegir el tamaño de página o si será fijo y proporciona a Copilot las directrices necesarias. Es posible que tengas que iterar, pedir cambios y aportar más contexto; es lo normal al trabajar con Copilot.

## Recursos

- [Modos Ask, Edit y Agent de Copilot: qué hacen y cuándo usarlos][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
