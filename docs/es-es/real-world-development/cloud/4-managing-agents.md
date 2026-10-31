---
title: "Lección 4: Supervisa y gestiona agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has asignado al agente de Copilot en la nube varias tareas para mejorar la experiencia y añadir funciones. Aunque trabaja de forma autónoma y asíncrona, es importante poder supervisar esas tareas.

Hay varias herramientas para gestionarlas, incluida la [página de agentes][agents-page] de GitHub.com. Desde Mission Control puedes consultar las tareas con PR abiertas, explorar las operaciones realizadas y orientar las sesiones en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- orientarás una sesión en curso para solicitar una función adicional.

## Escenario

Después de asignar el modo de alto contraste, el equipo ha decidido que también convendría añadir un modo claro. Como ya se están actualizando los estilos y añadiendo controles de selección, pedirás al agente que incluya también el modo claro.

## Revisa las tareas del agente en la nube

Consulta el estado de las tareas asignadas al agente de Copilot en la nube.

1. Abre la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Revisa las tareas de los paneles principal e izquierdo. Deberías encontrar las que has asignado a Copilot, entre ellas:
   - actualizar la documentación del código.
   - generar API para modificar juegos.
   - añadir un modo de alto contraste al sitio.
3. Selecciona una tarea en curso y revisa las operaciones realizadas por Copilot. Pueden incluir:
   - clonar el código del repositorio.
   - crear el entorno de trabajo.
   - configurar servidores MCP.
   - realizar los pasos necesarios para completar la tarea.

> [!NOTE]
> Los pasos concretos varían según el estado del trabajo de Copilot y el enfoque que haya seguido.

4. Observa el panel de PR que aparece a la derecha. Permite ver la solicitud y los archivos modificados.

## Orienta al agente en la nube

Pide a Copilot que añada el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre que haya elegido Copilot y del estado de la tarea.

    ![Sesión de accesibilidad en Mission Control](../../../_images/ex6-accessibility-session.png)

2. Espera unos minutos hasta que haya terminado la configuración y empezado a trabajar. Verás mensajes que indican el progreso.
3. En el cuadro **Steer active session while Copilot is working**, escribe este prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Tarea del agente en la nube en la página de agentes con el cuadro para orientar la sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot lo confirma y lo incorpora al flujo de trabajo.

## Deja que Copilot trabaje

Copilot incorporará la nueva solicitud cuando termine el paso en curso. El proceso puede tardar varios minutos; puedes aprovechar ese tiempo para repasar lo aprendido.

## Resumen y siguientes pasos

Has explorado la página de agentes de Copilot, el centro para supervisar y orientar las tareas del agente de Copilot en la nube. Permite seguir las tareas activas y completadas, revisar el trabajo y redirigir una sesión en curso para ajustar el alcance o aportar instrucciones.

Has aprendido a:

- usar Mission Control y la página de agentes para supervisar tareas.
- redirigir una sesión en curso y solicitar otra función.

Mientras Copilot termina el trabajo de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Itera sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
