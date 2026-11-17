---
title: "Ejercicio 4 - Supervisa y gestiona agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has pedido al agente en la nube de Copilot que se ocupe de tres tareas distintas para mejorar la experiencia de usuario y añadir funcionalidades. Aunque está diseñado para trabajar de forma autónoma y asíncrona, es importante poder supervisar esas tareas.

Puedes gestionar las tareas asignadas al agente mediante varias herramientas, incluida la [página de agentes][agents-page] de GitHub.com. Desde este centro puedes ver las tareas de los agentes con solicitudes de incorporación de cambios abiertas, consultar las operaciones realizadas y orientar las sesiones que aún están en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- orientarás una sesión en curso para solicitar otra funcionalidad.

## Escenario

Después de asignar al agente la creación de un modo de alto contraste, el equipo decide añadir también un modo claro. Como ya está actualizando los estilos y añadiendo controles de cambio de modo, es un buen momento para incluirlo. Orientarás al agente para que añada ambos modos.

## Revisa las tareas del agente en la nube

Consulta el estado de las tareas asignadas al agente en la nube de Copilot.

1. Abre la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Revisa la lista de tareas del panel principal y del panel izquierdo. Deberías ver las tareas asignadas a Copilot, entre ellas:
   - actualizar la documentación del código base.
   - generar API para modificar juegos.
   - añadir un modo de alto contraste al sitio web.
3. Selecciona una tarea en curso y revisa las operaciones realizadas por Copilot. Pueden incluir:
   - obtener el código del repositorio.
   - preparar el entorno de trabajo.
   - configurar servidores MCP.
   - realizar tareas necesarias para completar la solicitud.

> [!NOTE]
> Los pasos concretos dependerán del estado del trabajo de Copilot y del enfoque que haya elegido.

4. Observa también el panel de solicitud de incorporación de cambios (PR), a la derecha, que permite consultar la solicitud y los archivos modificados.

## Orienta al agente en la nube

Ahora que has visto las tareas activas, pide a Copilot que añada el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre que haya elegido Copilot y del estado del trabajo.

    ![Sesión de accesibilidad en Mission Control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que termine la configuración y empiece a trabajar. Verás mensajes que indican que el agente ha comenzado la tarea.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe este prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Tarea del agente en la nube con el cuadro de diálogo para orientar la sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot confirma que lo ha recibido y lo incorpora al flujo de trabajo.

## Deja que Copilot trabaje

Copilot incorporará la nueva solicitud al flujo cuando termine el paso que estaba ejecutando al recibir el mensaje. La tarea puede tardar varios minutos; puedes aprovechar ese tiempo para repasar lo aprendido.

## Resumen y siguientes pasos

Has explorado la página de agentes de Copilot, el centro para supervisar y orientar las tareas del agente en la nube. Permite seguir las tareas activas y completadas, revisar el trabajo y modificar las instrucciones de una sesión en curso.

Has explorado estos conceptos:

- has usado Mission Control y la página de agentes para supervisar las tareas.
- has redirigido una sesión en curso para solicitar más funcionalidades.

Mientras Copilot completa el trabajo de accesibilidad, [iterarás sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iteración sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
