---
title: "Ejercicio 5 - Supervisa y gestiona agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

En el modo de agente, GitHub Copilot trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Copilot Chat en VS Code muestra en tiempo real las llamadas a herramientas y las ediciones. Puedes revisar cada diff, aceptar o rechazar los cambios y orientar la conversación con prompts de seguimiento sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás los cambios propuestos.
- orientarás la sesión para añadir un control del modo claro además del modo de alto contraste.

## Escenario

El agente personalizado de accesibilidad del [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño solicita añadir también un modo claro. En lugar de iniciar otra conversación, orientarás la sesión actual para ampliar el trabajo.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra los pasos del agente: archivos que explora, comandos que ejecuta y cambios que propone. Las ediciones aparecen en el chat con un diff y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para observar las llamadas a herramientas, lecturas y ediciones de archivos y comandos de terminal.
3. Cuando proponga una edición, revisa el diff:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para descartarlo y permitir que el agente vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed** en la parte superior del chat para ver todos los archivos modificados durante la sesión.

> [!NOTE]
> Si una llamada a una herramienta necesita permiso, por ejemplo, para ejecutar un comando de terminal, el agente se detendrá y mostrará **Allow**. Aprueba solo los comandos que consideres seguros.

5. Espera a que el agente termine de implementar el modo de alto contraste. Debería mostrar un mensaje con un resumen de los cambios.

## Orienta al agente para añadir el modo claro

Cuando esté implementado el modo de alto contraste, amplía la misma conversación con una opción de modo claro. El agente conserva el contexto de los componentes, los estilos y el uso de almacenamiento local.

1. Envía este prompt de seguimiento desde Copilot Chat:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. El agente revisará sus cambios anteriores y propondrá los cambios para el modo claro.
3. Revisa cada edición y selecciona **Keep** o **Undo** para aceptarla o descartarla.
4. Si el agente sigue un enfoque que no quieres —por ejemplo, intenta reorganizar los controles—, detenlo con **Stop**, junto al campo de chat, y envía una aclaración.

> [!TIP]
> Orientar una sesión en curso puede ser más rápido que empezar de nuevo. Usa prompts de seguimiento para solicitar ajustes, corregir el enfoque o ampliar el alcance; por ejemplo, simplificar el CSS, integrar el modo claro en los controles existentes o añadir atajos de teclado.

## Revisa los cambios

Antes de confirmar el trabajo, revisa todos los archivos modificados.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa de archivos. Debería incluir componentes Astro, estilos y las pruebas relacionadas.
3. Abre algunos archivos y revisa los diffs. Confirma que se aplican los patrones de accesibilidad del agente: roles y atributos de Accessible Rich Internet Applications (ARIA), navegación con teclado, lenguaje de marcado de hipertexto (HTML) semántico y persistencia en almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la lección siguiente][next-lesson].

## Resumen y siguientes pasos

Has usado el modo de agente de Copilot Chat para supervisar y orientar una sesión en curso: has revisado cada edición, aceptado o rechazado cambios y enviado prompts de seguimiento para ampliar el trabajo sin perder contexto.

En esta lección:

- has supervisado el trabajo de un agente en tiempo real.
- has aceptado y rechazado ediciones de archivos.
- has orientado una sesión en curso con prompts de seguimiento.

A continuación, publicarás la funcionalidad de accesibilidad en una solicitud de incorporación de cambios y revisarás los cambios creados durante el taller.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Usa el modo de agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iteración sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
