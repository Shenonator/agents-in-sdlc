---
title: "Lección 2: Agente de Copilot en la nube"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que muchas organizaciones tengan deuda técnica: incidencias de seguridad pendientes, código heredado que necesita actualizarse o solicitudes de funcionalidad relegadas en el backlog por falta de tiempo. El agente de GitHub Copilot en la nube puede actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, crea una PR preliminar para que la revise un desarrollador. Así puedes delegar tareas tediosas, acelerar el desarrollo y centrarte en el trabajo de mayor alcance.

Con el agente de Copilot en la nube explorarás:

- cómo personalizar el entorno de generación de código.
- cómo realizar operaciones de forma segura.
- por qué es importante definir incidencias con un alcance claro.
- cómo asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica que quiere resolver. La primera versión del sitio se creó sin documentación y el equipo quiere añadir comentarios TSDoc a todas las funciones exportadas.

Además, el equipo de diseño quiere mejorar el descubrimiento de juegos. Quiere mostrar juegos relacionados —otros títulos de la misma categoría— en la página de detalles de cada juego para hacer pruebas de aceptación de la experiencia. No necesita una implementación refinada todavía; esta tarea bloquea las pruebas, pero hay incidencias de mayor prioridad.

Son tareas que suelen aplazarse y que se pueden asignar al agente de Copilot en la nube. El agente trabaja de forma asíncrona mientras el desarrollador se ocupa de otras tareas y, después, puede revisar el resultado.

## Presentación del agente de GitHub Copilot en la nube

El [agente de GitHub Copilot en la nube](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano de forma similar a un desarrollador. Por ejemplo, puedes [asignarle una incidencia de GitHub](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Después, Copilot crea una solicitud de incorporación de cambios preliminar para registrar el progreso, configura el entorno y empieza a trabajar. Puedes consultar la sesión durante el proceso o cuando haya terminado. Cuando la solución esté lista para revisarse, Copilot te mencionará en la PR.

## La importancia de definir bien las tareas

GitHub Copilot no hace innecesaria la planificación. Incluso las tareas aparentemente sencillas pueden ser complejas; por eso, presta atención a cómo delimitas el trabajo del agente de Copilot en la nube y sigue las [prácticas recomendadas][cloud-agent-best-practices]. Trátalo como a un compañero de programación con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. Los fundamentos del desarrollo de software no cambian por usar IA generativa.

## Instrucciones personalizadas de este repositorio

En los ejercicios anteriores has conocido las instrucciones personalizadas y cómo orientan a Copilot. Antes de asignar trabajo al agente en la nube, revisa los archivos de instrucciones del repositorio en modo de solo lectura para poder reconocer su efecto más adelante.

Abre estos archivos en la interfaz web de GitHub o en un Codespace:

- `.github/copilot-instructions.md`: revisa **Code standards**, en especial las convenciones de TypeScript y los comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observa el frontmatter `applyTo`, que aplica estas instrucciones a los archivos `**/*.test.ts`.

En el siguiente apartado, al asignar a Copilot la incidencia `Code lacks documentation`, fíjate en los comentarios TSDoc, las convenciones de TypeScript y los comentarios iniciales que aparezcan en la PR. Proceden de estos archivos de instrucciones; los volverás a revisar en un ejercicio posterior.

## Configura el entorno de desarrollo del agente en la nube

Desarrollar código suele requerir configurar un entorno y ejecutar scripts de preparación. Lo mismo ocurre cuando asignas tareas a Copilot, que trabaja de forma similar a un ingeniero de software.

El agente en la nube utiliza [GitHub Actions][github-actions] para configurar su entorno. Puedes personalizarlo con un [flujo de trabajo de configuración][setup-workflow] en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que el agente empiece. Este archivo ya está configurado para el taller y proporciona Node.js, las dependencias del proyecto, los navegadores para las pruebas de extremo a extremo y la base de datos SQLite migrada y precargada de la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
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

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier flujo de trabajo de GitHub, pero hay dos aspectos importantes:

- Contiene un único trabajo llamado `copilot-setup-steps`, que se ejecuta en GitHub Actions antes de que Copilot trabaje en la PR.
- El desencadenador `workflow_dispatch` permite ejecutar manualmente el flujo desde la pestaña **Actions** del repositorio. Es útil para comprobar que funciona sin esperar a que Copilot lo inicie.

## Añade documentación

Muchos proyectos tienen documentación desactualizada o carecen de ella. Esta deuda técnica reduce la productividad y dificulta mantener el código o incorporar a nuevos desarrolladores. Copilot puede generar documentación, por lo que esta es una buena tarea para asignarle. Más adelante revisarás el resultado.

1. En una nueva pestaña del navegador, abre el repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue**.
4. Selecciona **Blank issue**.
5. Escribe `Code lacks documentation` en **Title**.
6. Escribe lo siguiente en **Description**:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

    ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

    ![Detalles de la asignación de Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecciona la pestaña **Pull Requests**.
11. Abre la PR nueva, que tendrá un título parecido a `[WIP]: Code lacks documentation`. Si aún no aparece, espera un momento y actualiza la página.
12. Al cabo de unos minutos, Copilot debería haber creado una lista de tareas.

> [!NOTE]
> La lista puede tardar varios minutos en aparecer mientras Copilot configura el entorno con el flujo de trabajo anterior, analiza el proyecto y determina cómo abordar la tarea.

13. Revisa la lista y las tareas previstas.
14. Desplázate por la cronología de la PR hasta ver que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona **View session**.

    ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la página para ver el indicador actualizado.

16. Recorre la sesión en directo y observa cómo Copilot explora el código, analiza el estado, planifica y crea código.

El proceso puede tardar varios minutos. Una ventaja del agente en la nube es que trabaja de forma asíncrona mientras tú te ocupas de otras tareas. Aprovecharás esa capacidad asignándole otra tarea y, mientras tanto, trabajarás en nuevas funcionalidades.

## Añade una sección de juegos relacionados a la página de detalles

Puedes dividir el trabajo: mientras te ocupas de una tarea, el agente trabaja en otra. Asigna al agente la sección de juegos relacionados que solicita el equipo de diseño.

1. Vuelve al repositorio en github.com.
2. Selecciona **Issues**.
3. Selecciona **New issue**.
4. Selecciona **Blank issue**.
5. Escribe `Show related games on the game details page` en **Title**.
6. Escribe lo siguiente en **Description**:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot**.

    ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Al poco tiempo, verás un emoji 👀 en el primer comentario de la incidencia para indicar que Copilot está trabajando en ella.

![Copilot muestra el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en la segunda solicitud. Como trabaja de forma similar a un ingeniero de software, no hace falta supervisarlo continuamente; revisa el resultado cuando termine. Ahora puedes crear y usar agentes personalizados.

## Resumen y siguientes pasos

En este ejercicio has explorado el [agente de GitHub Copilot en la nube][copilot-agents]. Puedes asignarle incidencias para abordar deuda técnica, crear funcionalidades o migrar código entre frameworks de forma asíncrona.

Has aprendido a:

- personalizar el entorno de generación de código.
- realizar operaciones de forma segura.
- definir incidencias con un alcance claro.
- asignar incidencias a Copilot.

Mientras el agente en la nube trabaja, puedes crear y usar agentes personalizados. El [agente de Copilot en la nube también admite servidores MCP][cloud-agent-mcp] e instrucciones personalizadas, que ya has explorado en módulos anteriores.

## Recursos

- [Acerca del agente de Copilot en la nube][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]

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
