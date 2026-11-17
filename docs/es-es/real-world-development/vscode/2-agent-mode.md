---
title: "Ejercicio 2 - Añade funcionalidades con el modo de agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso los cambios sencillos suelen implicar editar varios archivos y ejecutar pruebas. Como desarrollador, normalmente tienes que localizar los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar los errores y repetir el proceso si falta algo. El modo de agente de Copilot puede explorar la estructura del proyecto, modificar varios archivos, ejecutar tareas como pruebas y corregir los errores que encuentre. En este ejercicio usarás el modo de agente para añadir una funcionalidad.

> [!NOTE]
> Aunque los nombres se parecen, el modo de agente y el agente en la nube ofrecen experiencias distintas. El modo de agente trabaja en tu IDE y permite ciclos de respuesta rápidos. El agente en la nube trabaja de forma asíncrona como un miembro del equipo y se coordina contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás a:

- hacer que el modo de agente explore el proyecto, identifique archivos relevantes y coordine los cambios.
- implementar funcionalidades en la interfaz y la capa de datos.
- revisar los cambios y las pruebas antes de combinarlos.

## Escenario

A medida que crece la lista de juegos, quieres permitir que los usuarios filtren por categoría y editorial. En el ejercicio anterior añadiste una función auxiliar para las editoriales; ahora completarás con el modo de agente el trabajo restante en la capa de datos, la interfaz y las pruebas.

## Ejecuta el sitio web Tailspin Toys

Antes de hacer cambios, explora el sitio para conocer sus funcionalidades actuales. Tailspin Toys es una plataforma de financiación colectiva de juegos de mesa con temática de desarrollo. Permite mostrar juegos y sus detalles. Es una sola aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle ORM; no hay una API de backend independiente ni un framework de interfaz en el cliente. Las funciones auxiliares reutilizables de acceso a datos están en `src/lib/`, y la interactividad se añade mediante un pequeño `<script>` de Astro con las API estándar del DOM.

### Inicia el sitio

La tarea `npm run dev` inicia el servidor de desarrollo de Astro.

1. Vuelve a Codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama para el filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor Astro esté listo, debería aparecer un mensaje parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir `http://localhost:4321` en un navegador, ciérralo seleccionando **x**.

5. Para abrir el sitio, mantén pulsada <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona `http://localhost:4321` en el terminal.

> [!NOTE]
> En Codespaces, al seleccionar el enlace localhost se redirige automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`, un túnel privado al servidor del Codespace.

### Explora el sitio

El sitio muestra una lista de juegos de mesa con títulos, imágenes y descripciones. Al seleccionar un juego, se abre una página de detalles con información adicional, como el título, la descripción, la editorial y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Vuelve al Codespace, abre Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; las instrucciones lo indicarán cuando sea necesario.

La plantilla creó un backlog de incidencias. Pide a Copilot que te muestre esas tareas.

1. En el menú de agentes de Chat, selecciona **Agent** para que Copilot planifique e implemente cambios, ejecute comandos de terminal y use herramientas.

   ![Selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Este taller no recomienda un modelo específico. Se usó Claude Sonnet 4.5 al prepararlo para que las sugerencias fueran relativamente coherentes, pero es normal que los resultados varíen porque los modelos de lenguaje grandes (LLM) son probabilísticos.

> [!NOTE]
> Copilot puede usar un comando MCP distinto, pero debería completar la tarea.

3. Envía este prompt:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para permitir que Copilot enumere las incidencias.
5. Revisa la lista generada y su priorización.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de [instrucciones de interfaz de usuario (UI)][ui-instructions], que proporciona contexto sobre cómo añadir funcionalidades al sitio.

1. Abre `.github/instructions/ui.instructions.md` en Codespaces.
2. Revisa la arquitectura, los principios de diseño de componentes, las pruebas y la accesibilidad. Observa los enlaces a instrucciones específicas de Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden enlazar otros archivos de instrucciones y archivos del proyecto. Las rutas son relativas al archivo que contiene el enlace, lo que permite reutilizar instrucciones y ejemplos.

## Implementa los filtros

Para completar el filtrado, tendrás que actualizar la lógica de datos en `src/lib/`, añadir o actualizar pruebas y modificar la página de lista de juegos para incluir la interfaz de filtrado. Copilot puede encargarse de estas tareas.

1. Continúa la conversación actual o selecciona **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui** para añadir el archivo de instrucciones de interfaz.

   ![Ejemplo de selección del archivo de instrucciones UI](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado.
4. Confirma que **Claude Sonnet 4.5** sigue seleccionado.
5. Pide a Copilot que implemente la funcionalidad según la incidencia:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto, localiza la capa de datos, la interfaz y las pruebas, y después modifica los archivos y ejecuta las pruebas.

   ![Copilot explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot puede tardar varios minutos en explorar, editar y ejecutar pruebas. Mientras trabaja, pueden aparecer **Keep** y **Undo** en el editor. No tienes que seleccionarlos hasta que Copilot termine.

7. Cuando Copilot te lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Cuadro de diálogo de Copilot Chat que pide confirmación para ejecutar las pruebas](../../../_images/ex3-agent-mode-run-tests.png)
8. Es normal que haya pausas o que fallen algunas pruebas durante el proceso; Copilot puede alternar entre generar código y ejecutar pruebas hasta completar la tarea.
9. Revisa el código generado para detectar problemas.

   ![Sesión completa de Copilot Agent Mode](../../../_images/ex3-agent-mode-proposed-changes.png)

> [!CAUTION]
> Revisa siempre el código generado por Copilot o cualquier otra herramienta de inteligencia artificial.

10. Vuelve al navegador donde se ejecuta el sitio y prueba los filtros.
11. Cuando hayas comprobado el funcionamiento y revisado el código, selecciona **Keep** en Copilot Chat.

## Resumen y siguientes pasos

Has usado el modo de agente de GitHub Copilot para añadir funcionalidades al sitio Tailspin Toys. Has aprendido a:

- explorar el proyecto, identificar archivos relevantes y coordinar cambios.
- implementar funcionalidades en la interfaz y la capa de datos.
- revisar los cambios y las pruebas antes de combinarlos.

A continuación, [probarás la funcionalidad con el servidor Playwright MCP][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Exploración adicional: implementa la paginación

Cuando crezca la lista de juegos, será necesario habilitar la paginación. Usa lo aprendido para pedir a Copilot que la implemente. Ten en cuenta lo siguiente:

- Sigue las prácticas recomendadas existentes, incluidos los archivos de instrucciones.
- Decide cómo debe funcionar la paginación y si el usuario podrá elegir el tamaño de página o si será fijo.
- En el prompt, proporciona a Copilot la orientación necesaria para implementar lo que quieres.
- Es posible que tengas que iterar con GitHub Copilot, pedir cambios y proporcionar contexto. Este proceso es habitual al trabajar con Copilot.

## Recursos

- [Modos Ask, Edit y Agent de Copilot: qué hacen y cuándo usarlos][choose-mode]
- [Modo de agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
