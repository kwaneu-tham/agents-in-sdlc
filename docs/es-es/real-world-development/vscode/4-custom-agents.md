---
title: "Ejercicio 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot son asistentes de inteligencia artificial especializados en tareas o ámbitos concretos del desarrollo. Puedes definirlos mediante archivos Markdown en `.github/agents` y proporcionar a Copilot instrucciones, prácticas recomendadas, patrones de código y conocimientos específicos. Por ejemplo, un agente de accesibilidad puede aplicar las pautas de accesibilidad para el contenido web (WCAG), uno de seguridad puede seguir prácticas de programación segura y uno de pruebas puede mantener patrones coherentes.

Los agentes personalizados se definen en archivos Markdown en `.github/agents` del proyecto o, de forma global, en `~/.copilot/agents`. Cada archivo tiene un frontmatter YAML con, al menos, los campos `name` y `description`, seguido de un prompt Markdown que define el comportamiento, la experiencia y las instrucciones del agente.

### Diferencias entre los agentes personalizados y las habilidades
Los agentes personalizados y las [habilidades de agente][agent-skills-concept] se parecen: ambos se definen principalmente mediante archivos Markdown e indican a una inteligencia artificial cómo realizar tareas. Una forma sencilla de distinguirlos es que el **agente personalizado** es quien realiza el trabajo y las **habilidades** son herramientas.
Los agentes personalizados tienen su propia ventana de contexto y pueden coordinar habilidades e incluso otros agentes. En este taller, el agente de accesibilidad revisará y actualizará el sitio y puede usar habilidades para gestionar solicitudes de incorporación de cambios o ejecutar pruebas.

> [!NOTE]
> No hay una única forma correcta de crear agentes personalizados. Prueba e itera para encontrar lo que funciona en tus entornos y escenarios.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills

## Escenario

Tailspin Toys quiere que su plataforma de financiación colectiva sea accesible para todas las personas. Algunos usuarios han indicado que el tema oscuro resulta difícil de leer porque el contraste entre el texto y el fondo es insuficiente. El equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar.

Usarás un agente personalizado para implementar esta funcionalidad de accesibilidad.

En este ejercicio:

- revisarás un agente personalizado de accesibilidad existente.
- usarás el agente de accesibilidad en Copilot Chat para implementar el modo de alto contraste.

## Revisa el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado de accesibilidad. Revisa su contenido para entender cómo orientará a Copilot.

Vuelve a Codespace, abre un terminal y crea una rama nueva a partir de `main` para mantener este trabajo separado de la solicitud de filtrado del ejercicio 3:

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abre `.github/agents/accessibility.md`.
2. Observa los campos `name` y `description` del frontmatter YAML.

> [!CAUTION]
> Los campos `name` y `description` del frontmatter son obligatorios para los agentes personalizados.

3. Revisa las secciones sobre las responsabilidades, las prácticas recomendadas de accesibilidad, ejemplos de lenguaje de marcado de hipertexto (HTML), hojas de estilo en cascada (CSS) y JavaScript, y errores habituales.

## Usa el agente personalizado en Copilot Chat

VS Code muestra en el menú de agentes de Copilot Chat los agentes personalizados definidos en `.github/agents`. Selecciona uno para aplicar sus instrucciones y herramientas a la sesión.

> [!TIP]
> **Abre Copilot Chat**
>
> Vuelve a Codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; las instrucciones lo indicarán cuando sea necesario.

1. En el menú de agentes de Chat, selecciona **Agent** si aún no está seleccionado.

   ![Selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Abre el menú de agentes en la parte inferior de la vista Chat. De forma predeterminada, muestra el agente activo **default**.
3. Selecciona **Accessibility agent**.
4. Envía al agente este prompt:

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat explorará el código, propondrá cambios y editará archivos. Cada edición aparecerá en el chat con la ruta del archivo y un diff que puedes revisar.
6. Observa el indicador **Files changed**, que se actualiza a medida que el agente modifica archivos.

> [!NOTE]
> El proceso puede tardar varios minutos. Copilot modifica archivos reales del repositorio, como componentes Astro, CSS y pruebas.

En el ejercicio siguiente revisarás y orientarás este trabajo mientras está en curso.

## Resumen y siguientes pasos

Has explorado los [agentes personalizados][custom-agents], asistentes de inteligencia artificial especializados en tareas y ámbitos concretos. Permiten convertir la experiencia y los estándares del equipo en agentes reutilizables que orientan el trabajo de Copilot.

Has explorado estos conceptos:

- cómo se definen los agentes personalizados.
- cómo usar un agente personalizado en el modo de agente de Copilot Chat.

A continuación, [supervisarás y orientarás el trabajo del agente][next-lesson], revisarás los cambios y añadirás un control para el modo claro en la misma sesión.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Crea agentes personalizados en tu IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados en Awesome Copilot][awesome-copilot-agents]

---

| [← Lección anterior: Prueba la funcionalidad con Playwright MCP][previous-lesson] | [Siguiente lección: Supervisión y gestión de agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
