---
title: "Lección 5: Supervisa y gestiona agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando usas el modo agente de GitHub Copilot, este trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Copilot Chat en VS Code muestra en tiempo real las llamadas a herramientas y las ediciones realizadas en tu equipo. Puedes revisar cada diferencia, aceptar o rechazar cambios y orientar la conversación con prompts adicionales sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás los cambios propuestos en los archivos.
- orientarás al agente durante la sesión para añadir un selector de tema claro junto al de alto contraste.

## Escenario

El agente personalizado de accesibilidad del [Ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño solicita además un selector de tema claro para quienes prefieren una interfaz menos oscura. En vez de empezar otra conversación, ampliarás el trabajo en la sesión actual.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: archivos que explora, comandos que ejecuta y cambios que propone. Cada edición aparece en el chat con las opciones **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del Ejercicio 4.
2. Mientras trabaja el agente, recorre el chat para ver cada llamada a herramienta, lectura, edición y comando de terminal.
3. Cuando proponga una edición, aparecerá la diferencia correspondiente:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para deshacerlo y permitir que el agente vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed** en la parte superior del chat para ver todos los archivos modificados durante la sesión.

> [!NOTE]
> Si una llamada a herramienta solicita permiso —por ejemplo, para ejecutar un comando de terminal—, el agente se detendrá y mostrará **Allow**. Aprueba solo los comandos que te parezcan adecuados.

5. Espera a que el agente termine de implementar el modo de alto contraste. Debería mostrar un mensaje con un resumen de los cambios.

## Pide al agente que añada el modo claro

Ahora que el modo de alto contraste está implementado, ampliarás la misma conversación para añadir un selector de tema claro. Como continuarás en la misma sesión, el agente conservará el contexto de los componentes, el enfoque de estilo y el patrón de almacenamiento local que ya utilizó.

1. Envía este prompt de seguimiento en Copilot Chat:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. El agente continuará desde los cambios existentes y propondrá los cambios necesarios para el modo claro.
3. Revisa cada cambio nuevo y selecciona **Keep** o **Undo**, como antes.
4. Si el agente sigue un enfoque que no quieres —por ejemplo, intenta reorganizar la interfaz de los selectores—, detenlo con el botón **Stop** junto al campo de chat y envíale una aclaración.

> [!TIP]
> Orientar una sesión en curso es más rápido que empezar de nuevo. Usa prompts de seguimiento para pedir ajustes, corregir el enfoque o ampliar el alcance; por ejemplo, puedes pedir que simplifique el CSS, integre el modo claro en el panel existente o añada atajos de teclado.

## Revisa los cambios

Antes de confirmar el trabajo, revisa todos los archivos que ha modificado el agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa de archivos modificados. Debería incluir componentes Astro, estilos y pruebas relacionadas.
3. Abre algunos archivos y revisa las diferencias. Comprueba que se aplican los patrones de accesibilidad del agente personalizado: atributos ARIA, navegación por teclado, HTML semántico y persistencia mediante almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios en el panel Source Control. Los publicarás en [la siguiente lección][next-lesson].

## Resumen y siguientes pasos

Has supervisado y guiado una sesión del modo agente de Copilot Chat: has revisado los cambios de cada archivo, aceptado o rechazado propuestas y enviado prompts de seguimiento para ampliar el trabajo sin perder contexto.

En esta lección has aprendido a:

- supervisar el trabajo de un agente en tiempo real.
- aceptar y rechazar ediciones individuales.
- orientar una sesión en curso con prompts de seguimiento.

En la [siguiente lección][next-lesson], publicarás el trabajo de accesibilidad como una solicitud de incorporación de cambios y revisarás los cambios del agente personalizado local.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Usar el modo agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Itera sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
