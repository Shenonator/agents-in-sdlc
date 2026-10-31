---
title: "Lección 5: Itera sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

## Revisa el trabajo

Durante este taller has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia y añadir funciones: documentar el código, crear una función de juegos relacionados y añadir opciones de accesibilidad con modos de alto contraste y claro. Revisa los cambios y, si hace falta, envía comentarios a Copilot para mejorarlos.

### Escenario

Los principios de diseño de software y DevOps no cambian al añadir IA generativa. Hay que revisar el código generado y seguir el proceso habitual. Revisarás las propuestas de Copilot para la documentación, los juegos relacionados y la accesibilidad antes de habilitar la revisión para el resto del equipo.

## Seguridad y agente de GitHub Copilot en la nube

Como el agente en la nube trabaja de forma asíncrona y sin supervisión, se aplican algunas restricciones de seguridad:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que utiliza para el código.
- El agente se ejecuta en GitHub Actions, en un entorno efímero independiente.
- Una persona debe aprobar los flujos de trabajo de GitHub Actions antes de ejecutarlos.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisa la documentación generada

Empieza por la primera solicitud de incorporación de cambios (PR) que generó el agente: la documentación del código. La revisarás desde la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Puede que la PR muestre una advertencia de que el cortafuegos ha bloqueado a Copilot. Es lo esperado: el acceso a recursos externos, incluidas las llamadas a servidores MCP, está limitado de forma predeterminada. Si quieres, puedes [personalizar o deshabilitar el cortafuegos del agente en la nube][agent-firewall].

1. Vuelve al repositorio en github.com.
2. Selecciona **Pull Requests**.
3. Abre la PR titulada, por ejemplo, **Add missing documentation**.

> [!NOTE]
> Si Copilot sigue trabajando, la PR tendrá la etiqueta **[WIP]**. Espera a que termine; puede tardar varios minutos.

4. Cuando la PR esté lista, selecciona **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que incluye comentarios TSDoc y otra documentación. Los cambios concretos pueden variar. Busca los comentarios TSDoc, las convenciones de TypeScript y los comentarios iniciales que se definieron en los archivos de instrucciones del Ejercicio 2.
6. Cuando hayas revisado los cambios, vuelve a **Conversation** y desplázate hacia abajo.
7. Comprueba si hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

    ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos aparecerán en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro se superarán; puede tardar varios minutos.

## Pide cambios a GitHub Copilot

Puedes mencionar a Copilot en comentarios de la PR o en comentarios en línea del código, igual que a otros miembros del equipo. Copilot verá el comentario e iniciará otra sesión para resolverlo. Como los resultados no son deterministas, no hay un texto único que debas usar. Puedes pedir, por ejemplo:

- añadir en cada archivo de código un comentario inicial que describa brevemente su función.
- añadir comentarios TSDoc a archivos TypeScript y Astro.
- crear un README que describa la estructura de la aplicación Astro.

1. Añade un comentario en la PR y menciona a **@copilot**. Usa una de las ideas anteriores u otra petición relacionada con la documentación.
2. Selecciona **View Session** para ver el trabajo de Copilot; iniciará una sesión nueva para aplicar los cambios.
3. Selecciona **Back to pull request** para volver a la PR.

    ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, aparecerá una confirmación nueva en la PR.
5. Selecciona **Files changed** y revisa los cambios.

Puedes seguir iterando hasta que estés conforme. Después, puedes marcar la PR como lista para revisión y combinarla.

![Marcar la PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la función de juegos relacionados

Vuelve a la PR que Copilot creó para mostrar juegos relacionados en la página de detalles.

1. Abre el repositorio en GitHub.com.
2. Selecciona **Pull Requests**.
3. Abre la PR titulada, por ejemplo, **Show related games on the game details page**.
4. Selecciona **Files changed** y revisa el código generado.
5. Cuando hayas terminado, vuelve a **Conversation** y desplázate hacia abajo.
6. Comprueba si hay flujos de trabajo pendientes de aprobación; si los hay, selecciona **Approve and run workflows**.

    ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

7. En la sección de comprobaciones de la PR, confirma que se han puesto en cola y que se superan las comprobaciones de la aplicación Astro. Puede tardar varios minutos.
8. **Opcional:** puedes cambiar al árbol de trabajo de esa rama en el Codespace para probar manualmente la función. Abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por la rama creada por Copilot, por ejemplo, **copilot/fix-8**:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot ha creado la función de juegos relacionados. Puedes seguir iterando y pedirle que cambie, por ejemplo, el número de juegos que se muestran. También puedes pedir que añada comentarios iniciales y comentarios TSDoc; recuerda que la incidencia se asignó **antes** de actualizar las instrucciones personalizadas. Añade una solicitud en **Conversation** y Copilot iniciará otra sesión.

## Revisa las funciones de accesibilidad

Por último, revisa la PR de las funciones de accesibilidad, que debe incluir el modo de alto contraste asignado en el Ejercicio 3 y el modo claro solicitado desde Mission Control en el Ejercicio 4.

1. Abre el repositorio en GitHub.com.
2. Selecciona **Pull Requests**.
3. Abre la PR titulada, por ejemplo, **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot sigue trabajando, la PR tendrá la etiqueta **[WIP]**. Espera a que termine; puede tardar varios minutos.

4. Selecciona **Files changed** y revisa el código.
5. Presta atención a:
   - los controles para cambiar de modo.
   - el uso del almacenamiento local para guardar las preferencias.
   - los cambios de CSS o estilo para los modos de alto contraste y claro.
   - los atributos de accesibilidad, como etiquetas ARIA y navegación por teclado.
   - el código JavaScript o TypeScript que gestiona los modos.
6. Vuelve a **Conversation** y desplázate hacia abajo.
7. Comprueba si hay flujos de trabajo pendientes de aprobación; si los hay, selecciona **Approve and run workflows**.

    ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. En la sección de comprobaciones de la PR, confirma que se han puesto en cola y que se superan las comprobaciones de la aplicación Astro.
9. **Opcional:** cambia a la rama en tu Codespace y prueba manualmente las funciones de accesibilidad. Sustituye `<branch-name>` por la rama que creó Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Después, inicia la aplicación y prueba los selectores de alto contraste y modo claro en el navegador. Confirma que funcionan y que las preferencias se conservan al recargar la página.

El agente personalizado de accesibilidad ha orientado a Copilot para seguir las prácticas recomendadas. Si detectas problemas o mejoras posibles, menciona a **@copilot** en un comentario de la PR, como hiciste con las anteriores.

## Ejercicio opcional: sigue delegando

El agente en la nube resulta útil para delegar tareas reales del backlog mientras te ocupas de otras. Para practicar, crea más incidencias y asígnaselas a Copilot. Algunas ideas:

- crear un formulario para indicar interés en un juego.
- añadir paginación a la lista de juegos.
- validar las entradas y gestionar errores en las funciones auxiliares de acceso a datos.

## Resumen

Has completado el recorrido del agente en la nube. En estas lecciones:

- has revisado los archivos de instrucciones incluidos en el repositorio para reconocer su efecto en el trabajo del agente.
- has asignado incidencias al agente de Copilot en la nube y observado cómo prepara el entorno, planifica y trabaja de forma asíncrona.
- has creado y usado un agente personalizado de accesibilidad para añadir los modos de alto contraste y claro.
- has utilizado la página de agentes de Copilot como Mission Control para supervisar y orientar la sesión.
- has revisado y mejorado las PR del agente, mencionando a `@copilot` para solicitar cambios y aprobando los flujos de trabajo.

## Repaso y siguientes pasos

Has completado el recorrido del agente en la nube. Para seguir aprendiendo, puedes explorar otros recorridos:

- 🖥️ **[Recorrido de VS Code](../../vscode/)**: explora el modo agente de Copilot Chat y la integración MCP desde el IDE.
- 💻 **[Recorrido de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con Plan y Autopilot, habilidades de agente, un agente de QA, Playwright MCP, Agent Merge y la opción de delegar tareas al agente en la nube con `/delegate`.

En tu propio repositorio, también puedes:

- asignar al agente una incidencia de refactorización o cobertura de pruebas.
- crear un agente personalizado para otro ámbito.
- configurar `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]
- [Configurar el cortafuegos del agente en la nube][agent-firewall]

---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
