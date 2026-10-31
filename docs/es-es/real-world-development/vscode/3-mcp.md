---
title: "Lección 3: Prueba la funcionalidad con el servidor Playwright MCP"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Modo agente][previous-lesson] · [Siguiente lección: Agentes personalizados →][next-lesson]

Acabas de crear el filtrado con el modo agente. Antes de abrir una solicitud de incorporación de cambios, comprueba que funciona en el navegador. En vez de recorrer la aplicación manualmente, conectarás el **servidor Playwright MCP** y permitirás que Copilot pruebe la funcionalidad en un navegador real. Después publicarás la rama y abrirás la PR.

En este ejercicio:

- aprenderás qué es Model Context Protocol (MCP) y cómo amplían los servidores MCP las herramientas de Copilot.
- añadirás el servidor Playwright MCP al espacio de trabajo.
- pedirás a Copilot que lo use para probar manualmente el filtrado en un navegador.
- publicarás la rama y abrirás una PR para el filtrado.

## ¿Qué es Model Context Protocol (MCP)?

El modo agente es más potente cuando puede acceder a recursos externos al editor. Model Context Protocol (MCP) permite que Copilot se comunique con herramientas y servicios externos mediante un protocolo estándar.

![Diagrama del funcionamiento interno del modo agente y su interacción con el contexto, el LLM y las herramientas, incluidas las proporcionadas por servidores MCP y extensiones de VS Code.](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que los agentes de IA se comuniquen con herramientas y servicios externos en tiempo real. Pueden acceder a información actualizada mediante recursos y realizar acciones en tu nombre mediante herramientas.

Se accede a estos elementos mediante un servidor MCP, que actúa como puente entre el agente y las herramientas externas, como API existentes o herramientas locales en paquetes npm. Cada servidor ofrece un conjunto distinto de herramientas y recursos.

Algunos servidores MCP populares son:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: proporciona API para gestionar repositorios de GitHub, crear repositorios y gestionar incidencias y solicitudes de incorporación de cambios.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: proporciona automatización del navegador con Playwright para navegar por páginas, rellenar formularios y seleccionar botones.

Hay muchos servidores MCP. GitHub aloja un [registro de MCP](https://github.com/mcp) para facilitar su descubrimiento y las contribuciones al ecosistema.

> [!CAUTION]
> Por seguridad, trata los servidores MCP como cualquier otra dependencia. Antes de usar uno, revisa su código fuente, verifica quién lo publica y considera las implicaciones de seguridad. Usa únicamente servidores de confianza y ten cuidado al conceder acceso a recursos u operaciones sensibles.

## Revisa la configuración MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en este espacio de trabajo de VS Code.

1. Abre `.vscode/mcp.json` en el Codespace.
2. Deberías encontrar un servidor `github` ya configurado:

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

La plantilla incluye esta entrada `github`, que permitió a Copilot leer el backlog en el ejercicio anterior. Usa el [servidor MCP remoto de GitHub][remote-github-mcp-server]. No tienes que instalarlo localmente: VS Code se conecta por HTTP y te autenticas con GitHub la primera vez que Copilot usa una de sus herramientas.

## Añade el servidor Playwright MCP

Añade un segundo servidor. [Playwright MCP][playwright-mcp-server] proporciona a Copilot un navegador que puede controlar para probar la funcionalidad.

1. Añade una entrada `playwright` junto a `github` en `.vscode/mcp.json`:

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

A diferencia del servidor `github` remoto, `playwright` es local: VS Code lo inicia en el equipo con `npx @playwright/mcp@latest`. La opción `--headless` ejecuta el navegador sin ventana visible, algo necesario en un Codespace, que no tiene escritorio gráfico.

> [!NOTE]
> Tailspin Toys ya utiliza Playwright para las pruebas de extremo a extremo, por lo que normalmente el navegador está instalado. Si Copilot indica que falta, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

## Inicia el servidor y confía en él

VS Code inicia los servidores MCP cuando se necesitan. La primera vez que Copilot necesita las herramientas de un servidor, VS Code lo inicia y te pide que confirmes que confías en él.

1. En `.vscode/mcp.json`, selecciona **Start**, que aparece encima de la entrada `playwright`, para iniciarlo y confirmar que la configuración es correcta.
2. Si VS Code te pregunta si confías en el servidor, revisa la configuración y confirma que confías en él.

Iniciarlo manualmente es opcional; si no lo haces, VS Code lo iniciará y te pedirá confirmación la primera vez que Copilot necesite sus herramientas.

## Prueba la funcionalidad de filtrado

El servidor [Playwright MCP][playwright-mcp-server] permite que Copilot controle un navegador real. El agente puede abrir una página, navegar, aplicar filtros y leer los resultados, en lugar de que tengas que hacer estas comprobaciones manualmente.

El servidor usa el [árbol de accesibilidad][playwright-mcp-server] de la página en vez de capturas de pantalla. El agente trabaja con elementos estructurados y etiquetados —botones, enlaces y elementos de listas— como lo haría una tecnología de asistencia. Así, una comprobación funcional rápida también sirve para revisar aspectos básicos de accesibilidad.

Con el servidor conectado y la aplicación en ejecución, pide a Copilot que pruebe el filtrado:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot abrirá un navegador mediante Playwright MCP, seguirá los pasos y te comunicará el resultado. Compara el resumen con los criterios de aceptación de la incidencia. Si algo no funciona, pregunta o pide corregir el código antes de abrir una PR.

> [!NOTE]
> Para esta prueba, la aplicación debe estar en ejecución en `http://localhost:4321`. Si has detenido el servidor de desarrollo, inícialo de nuevo antes de enviar el prompt. La primera vez que Copilot use Playwright MCP puede tener que descargar un navegador; si indica que falta, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publica la rama y crea una solicitud de incorporación de cambios

Ya has comprobado que la funcionalidad funciona. Ahora abrirás una solicitud de incorporación de cambios (PR) para que el equipo la revise. Primero, publica la rama `filtering-vscode`.

1. Abre el panel **Source Control** del Codespace y revisa los cambios de Copilot.
2. Prepara los cambios seleccionando el icono **+**.
3. Genera un mensaje de confirmación con el botón **Sparkle**.

   ![Panel Source Control con los cambios realizados.](../../../_images/ex3-source-control-changes.png)

4. Selecciona **Publish** para enviar la rama al repositorio.

## Crea la solicitud de incorporación de cambios

Hay varias formas de crear una PR, incluida la interfaz de GitHub y GitHub CLI. También puedes pedir a Copilot que encuentre la incidencia adecuada y cree una PR asociada.

1. Abre Copilot Chat y selecciona **New Chat** para iniciar una sesión nueva.
2. En el selector de agentes, confirma que está seleccionado **Agent** para que Copilot pueda usar las herramientas de GitHub.
3. Pide a Copilot que cree la PR:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Si es necesario, selecciona **Continue** para permitir que Copilot recopile información y ejecute las acciones. La primera vez que use una herramienta de GitHub, quizá debas autenticarte.
5. Observa cómo Copilot busca la incidencia correcta y crea la PR.
6. Selecciona el enlace de Copilot para revisar la PR, pero **no la combines todavía**.

## Resumen y siguientes pasos

En este ejercicio:

- has aprendido qué es Model Context Protocol (MCP) y cómo amplían los servidores MCP las herramientas de Copilot.
- has añadido Playwright MCP al espacio de trabajo.
- lo has usado para probar manualmente el filtrado en un navegador antes de publicarlo.
- has publicado la rama y abierto una PR para el filtrado.

Has usado un servidor MCP para probar una funcionalidad, pero MCP es solo una forma de ampliar Copilot. A continuación, [crearás un agente personalizado][next-lesson] para agilizar tareas específicas, como las revisiones de accesibilidad.

## Recursos

- [Qué es MCP y por qué todo el mundo habla de ello][mcp-blog-post]
- [Servidor MCP de Microsoft Playwright][playwright-mcp-server]
- [Servidor MCP de GitHub][github-mcp-server]
- [Registro MCP de GitHub][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
