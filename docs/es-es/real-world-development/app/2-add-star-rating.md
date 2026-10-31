---
title: "Lección 2: Añade valoraciones por estrellas: una mejora rápida"
description: "Inicia tu primera sesión de agente en la aplicación GitHub Copilot, haz un pequeño cambio en las tarjetas de juegos y combínalo en tu primera solicitud de incorporación de cambios."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Ya has recorrido el espacio de trabajo y usado un chat rápido. Ahora iniciarás una **sesión de agente** y harás tu primer cambio en el proyecto. Será pequeño: los juegos ya incluyen una valoración por estrellas en sus datos, pero las tarjetas de la página principal no la muestran. Pedirás al agente que la muestre, revisarás el cambio y lo combinarás en tu primera solicitud de incorporación de cambios.

En esta lección:

- iniciarás una sesión de agente y aprenderás cómo se estructura.
- pedirás al agente que haga un cambio pequeño y específico en el proyecto.
- revisarás el cambio en la vista de diferencias del espacio de trabajo.
- ejecutarás la aplicación en local y confirmarás el cambio en el navegador.
- abrirás y combinarás tu primera solicitud de incorporación de cambios.

## Escenario

Cada juego de Tailspin Toys puede tener una valoración por estrellas, que ya aparece en la página de detalles. Sin embargo, las tarjetas de la página principal solo muestran el título, la categoría, la editorial y la descripción. Como ejercicio inicial, pedirás al agente que muestre la valoración existente en cada tarjeta: un cambio pequeño y autónomo, ideal para tu primera sesión.

## Anatomía de una sesión

Una **sesión** es una conversación con un agente que se ejecuta en su propio espacio de trabajo aislado. Cada sesión dispone de un **árbol de trabajo y una rama de Git dedicados**, lo que permite ejecutar varias sesiones a la vez —por ejemplo, una para añadir una funcionalidad y otra para corregir un error— sin que sus cambios entren en conflicto. Las sesiones aparecen en la barra lateral, agrupadas por repositorio; selecciona una para cambiar a ella.

En una sesión verás tres elementos: la **conversación** con el agente, la **actividad de las herramientas** mientras explora y edita archivos, y la lista de **archivos modificados** con sus diferencias.

## Inicia una sesión y solicita el cambio

Inicia una sesión nueva para explorar el proyecto e implementar la funcionalidad. Durante la [configuración de la aplicación][prior-lesson], añadiste el proyecto desde su repositorio de GitHub. Ahora crearás una sesión para ese repositorio y solicitarás el cambio.

1. Vuelve a la aplicación GitHub Copilot o ábrela.
2. Selecciona **+** en la parte superior de la barra lateral para iniciar una sesión.
3. Selecciona `tailspin-toys` como repositorio.
4. Elige **new working tree** y el modo **Interactive** debajo del cuadro del prompt. Usa este prompt:

    ```plaintext
    Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
    ```

5. Pulsa <kbd>Enter</kbd> para enviar el prompt a Copilot.

La aplicación Copilot empieza creando un árbol de trabajo nuevo, una copia aislada del proyecto. Después explora el proyecto, localiza los archivos que debe modificar y crea el código necesario.

## Revisa las diferencias

Revisa todos los cambios generados por IA antes de combinarlos, por pequeños que sean.

1. En la esquina superior derecha, selecciona **Toggle review panel**. En el panel que aparece, selecciona **Changes** para ver los archivos modificados y sus diferencias.

    ![Barra de herramientas de la aplicación GitHub Copilot con una flecha que señala el botón Toggle review panel, a la derecha de Create PR.](../../../_images/app-2-review-panel.png)

2. Deberías ver código añadido a `GameCard.astro`, el archivo principal para mostrar los detalles del juego. Debería parecerse a este bloque, que muestra la valoración cuando existe y, si `starRating` es `null`, indica que aún no hay valoración:

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> Como todas las herramientas de IA generativa, Copilot es probabilístico, no determinista; el código exacto puede variar. Debería ser parecido al ejemplo.

## Comprueba los cambios

Revisa los resultados de las comprobaciones automatizadas del agente antes de abrir un navegador. Confirma que las pruebas cubren una valoración `starRating` numérica y el caso alternativo `null`. Si falta un requisito previo o se ha omitido una comprobación, no la des por superada; revisa cualquier solicitud de instalación antes de aprobarla.

Pide a Copilot que abra el sitio web para examinar la interfaz actualizada en un lienzo del navegador.

> [!TIP]
> Un lienzo es un widget interactivo integrado en la aplicación Copilot. Más adelante explorarás lienzos personalizados e incluso crearás uno, pero ahora usarás el lienzo de navegador integrado.

1. Envía este prompt para pedir a Copilot que inicie la aplicación y abra la página en el lienzo de navegador:

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. En unos instantes, se iniciará la aplicación y se abrirá el navegador dentro de Copilot.
3. Confirma que las tarjetas de juegos valorados muestran su puntuación sobre cinco.
4. Cuando termines, pide a Copilot que detenga el servidor de desarrollo y cierre el lienzo del navegador:

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## Abre y combina tu primera solicitud de incorporación de cambios

Ya has creado la funcionalidad. Ahora abrirás una solicitud de incorporación de cambios (PR) para integrarla en el código existente.

1. Selecciona **Create PR** en la esquina superior derecha.
2. Si se te solicita, selecciona **Sign in with your browser** y sigue las instrucciones para autenticarte.
3. Si Copilot indica que la política del repositorio requiere una incidencia relacionada antes de crear la PR, confirma que debe seguir ese proceso.
4. Copilot creará la PR.
5. Selecciona la burbuja **PR** encima del chat para abrir la solicitud en el panel de revisión.
6. Cuando esté lista, selecciona **Ready to merge**.
7. En el nuevo cuadro de diálogo, selecciona **Merge pull request** para combinarla.

## Resumen y siguientes pasos

Has publicado tu primer cambio con la aplicación GitHub Copilot. En concreto:

- has iniciado una sesión de agente y aprendido cómo se estructuran las sesiones.
- has dirigido al agente para que haga un cambio pequeño y específico en las tarjetas de juegos.
- has revisado el cambio en la vista de diferencias del espacio de trabajo.
- has ejecutado la aplicación en local y confirmado la valoración por estrellas en el navegador.
- has abierto la PR 1, revisado sus comprobaciones y la has combinado explícitamente.

A continuación, [partirás de la incidencia de filtrado y usarás los modos Plan y Autopilot][next-lesson] para crear una funcionalidad más amplia.

## Recursos

- [Trabajar con sesiones de agente en la aplicación GitHub Copilot][agent-sessions]
- [Acerca de la aplicación GitHub Copilot][about-copilot-app]
- [Gestionar incidencias y solicitudes de incorporación de cambios con la aplicación GitHub Copilot][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#instalar-y-configurar-la-aplicación-github-copilot
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
