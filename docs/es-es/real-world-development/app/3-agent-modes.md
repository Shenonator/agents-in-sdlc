---
title: "Lección 3: Modos de agente: Plan y Autopilot"
description: "Explora los modos de agente: usa Plan para acordar un enfoque, Autopilot para crear el filtrado a partir de una incidencia e Interactive para revisar y verificar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Empezamos añadiendo una pequeña funcionalidad al proyecto, pero los cambios más amplios requieren un proceso más sólido. La aplicación GitHub Copilot está diseñada para adaptarse al flujo de trabajo de una organización y ayudar a crear lo adecuado de la manera correcta. En esta y otras lecciones seguirás un proceso habitual de desarrollo con agentes: partirás de una incidencia para crear una funcionalidad, comprobarás el código y su comportamiento y, finalmente, la integrarás en el proyecto.

> [!NOTE]
> Usarás la misma sesión durante todo el flujo de trabajo de la funcionalidad. Normalmente se usarían sesiones o PR distintas para los diferentes tipos de archivos, pero seguiremos un atajo para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una sesión de agente desde una incidencia de GitHub.
- definirás los requisitos en modo **Plan**.
- implementarás la funcionalidad con el modo **Autopilot**.
- revisarás el código.
- validarás manualmente la funcionalidad en un lienzo del navegador.

Al continuar con esta funcionalidad, actualizarás las instrucciones del repositorio, personalizarás la habilidad existente `quality-checks`, añadirás validación con MCP, crearás un agente de QA y abrirás la PR de la funcionalidad.

## Escenario

El catálogo de Tailspin Toys está creciendo y sus visitantes necesitan filtrar los juegos por categoría y editorial. La incidencia del backlog describe la funcionalidad, pero hay detalles —como la combinación de categorías— que deben acordarse antes de programar. Usarás el modo Plan para resolver esas decisiones y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

Incorporar agentes de programación con IA al flujo de desarrollo no cambia los principios fundamentales; de hecho, los hace aún más importantes. Un flujo habitual es:

1. Abrir una incidencia registrada que describa el trabajo.
2. Crear un plan de lo que se debe desarrollar.
3. Crear y revisar el código.
4. Ejecutar pruebas para validar el código.
5. Validar manualmente la nueva funcionalidad.
6. Crear una solicitud de incorporación de cambios (PR).
7. Combinar el código cuando se haya revisado y se hayan superado las comprobaciones de integración continua.

> [!NOTE]
> Los detalles concretos dependen del equipo y la organización, pero la mayoría de los procesos siguen alguna variante de estos pasos.

Este enfoque ayuda a garantizar que el código generado por IA cumple los requisitos y pasa por las mismas revisiones que el código escrito manualmente.

## Modos de sesión

El **modo de sesión** controla la autonomía del agente. Puedes seleccionarlo en el menú desplegable situado debajo del campo del prompt y cambiarlo en cualquier momento:

- **Interactive**: trabajas junto con el agente. Propone cambios y espera tu respuesta antes de continuar.
- **Plan**: el agente crea primero un plan. Lo revisas y apruebas antes de que el agente lo ejecute.
- **Autopilot**: el agente trabaja de forma autónoma: escribe código, ejecuta pruebas e itera sin esperar indicaciones.

Empieza en modo Plan, revisa el plan y, después, usa Autopilot para implementarlo.

## Inicia una sesión desde la incidencia

Antes de empezar, confirma que la PR de valoraciones por estrellas está combinada y que tu `main` local está actualizado.

1. En **Sessions**, selecciona la carpeta del repositorio Tailspin Toys y, después, **Issues** en la navegación superior del repositorio. Abre **Allow users to filter games by category and publisher**. Es una incidencia de ejemplo existente; no tienes que crear otra.
2. Selecciona **New session** y elige un **new working tree** basado en `main` actualizado.

   ![Vista de una incidencia en la aplicación GitHub Copilot con una flecha que señala el botón New session.](../../../_images/app-new-session-from-issue.png)

3. Confirma que la incidencia está adjunta a la sesión y selecciona **Plan** en el selector de modo.

> [!NOTE]
> Si no se encuentra Node.js o npm, pide al agente que ejecute `node --version` y `npm --version`. Si funcionan en el terminal pero no en la sesión, comprueba `PATH` o la configuración del gestor de versiones de la aplicación y ciérrala por completo antes de volver a abrirla. Reintenta la operación y las comprobaciones fallidas; no reinstales npm solo porque el agente no lo encuentre.

## Planifica la funcionalidad de filtrado

La planificación permite revisar el enfoque antes de que Copilot escriba código. Como has iniciado la sesión desde la incidencia, Copilot ya tiene la solicitud como contexto. Envía:

```plaintext
Build this feature.
```

Responde a las preguntas de Copilot y compara el plan con los criterios de aceptación de la incidencia. Comprueba que incluye el filtrado por categoría y editorial, controles accesibles, cambios de acceso a datos y pruebas. Aclara cualquier comportamiento poco definido, como la combinación de varias categorías o qué ocurre cuando ningún juego coincide.

El plan debe incluir lint, pruebas unitarias, pruebas E2E y comprobación de tipos con las herramientas existentes del proyecto. Céntralo en implementar y probar el filtrado; crearás la PR tras completar el flujo de calidad. Solicita cambios en el plan antes de aprobarlo y conserva la URL de la incidencia y las aclaraciones acordadas para validarlas más adelante.

## Aprueba Autopilot explícitamente

Cuando estés conforme con el plan, selecciona **Approve and implement this plan** o la opción equivalente de tu versión. Comprueba el modo activo en vez de dar por hecho que la aprobación cambia el modo. El indicador debe mostrar **Autopilot**.

Copilot comenzará a trabajar en la implementación: seguirá el plan, generará código y ejecutará pruebas.

> [!NOTE]
> La aprobación puede iniciar la implementación inmediatamente, así que revisa el plan primero. Si Copilot informa de dependencias ausentes o de un conflicto de puertos, resuelve el problema de configuración antes de dar las comprobaciones por completadas. Detén solo los servidores que hayas iniciado.

## Revisa y verifica la implementación

Como cualquier otro código, el código generado debe revisarse antes de combinarlo. Revisa los cambios y ejecuta el sitio.

1. Abre **Changes** e inspecciona la implementación del filtrado y las pruebas.
2. Compara el resultado con la incidencia y las aclaraciones aprobadas, incluidas las combinaciones de varias categorías y editoriales. Comprueba que los cambios siguen las instrucciones del repositorio.
3. Revisa los resultados de lint, las pruebas unitarias, las pruebas E2E y la comprobación de tipos. Una comprobación omitida no cuenta como superada.
4. Resuelve los fallos y vuelve a ejecutar las comprobaciones afectadas antes de aceptar la implementación. La configuración E2E de Playwright compila y sirve una vista previa y puede reutilizar un servidor local; confirma que el servidor probado pertenece a este árbol de trabajo, no a una lección anterior.

## Explora la nueva funcionalidad

El código parece correcto, pero ¿se ejecuta? Inicia la aplicación como antes y abre el sitio en un lienzo de navegador.

1. Usa este prompt para pedir a Copilot que inicie la aplicación y abra la página en el lienzo:

   ```plaintext
   Start the app and open it in the browser canvas.
   ```

2. En unos instantes, se iniciará la aplicación y aparecerá un navegador dentro de Copilot.
3. Selecciona una categoría y confirma que los resultados coinciden. Borra el filtro, selecciona una editorial y comprueba los resultados.
4. Combina los filtros de categoría y editorial y compara los resultados con la incidencia y el plan aprobado. Si se admiten varias categorías, comprueba el comportamiento acordado.
5. Prueba una combinación sin resultados y comprueba el estado vacío. Borra o restablece los filtros y confirma que vuelve a aparecer la lista completa.
6. Cuando termines, pide a Copilot que detenga el servidor y cierre el lienzo:

   ```plaintext
   Stop the dev server and close the browser canvas.
   ```

## Conserva esta sesión para las lecciones siguientes

Todavía no crees ni combines la PR del filtrado. Conserva esta sesión, el árbol de trabajo y la rama para las lecciones 4 a 8. A continuación, aplicarás instrucciones personalizadas al código de filtrado y añadirás mejoras a la habilidad y al agente de QA antes de crear y combinar la PR conjunta en la Lección 8. Después de detener el servidor, puedes dejar la sesión inactiva y volver a ella en la siguiente lección.

## Resumen y pasos siguientes

Has usado distintos modos de agente para crear y revisar una funcionalidad. En esta lección:

- has iniciado una sesión de agente desde una incidencia de GitHub.
- has definido los requisitos en modo **Plan**.
- has implementado la funcionalidad con **Autopilot**.
- has revisado el código.
- has validado manualmente la funcionalidad en un lienzo de navegador.

A continuación, profundizarás en cómo se genera el código y te asegurarás de que siga las prácticas documentadas mediante el [uso de instrucciones personalizadas][next-lesson].

## Recursos

- [Trabajar con sesiones de agente en la aplicación GitHub Copilot][agent-sessions]

[next-lesson]: ../4-custom-instructions/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
