---
title: "Ejercicio 2 - Añade funcionalidad con el modo de agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y tareas como ejecutar pruebas. Normalmente, el flujo de trabajo de un desarrollador consiste en buscar todos los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo falta, volver a modificarlo… y así sucesivamente.

El modo de agente de Copilot ayuda a resolver este problema.

El modo de agente de Copilot está diseñado para trabajar de forma más autónoma en el IDE. Se comporta de manera parecida a un desarrollador: primero explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentra. Veamos cómo usar el modo de agente para añadir funciones al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo de agente y el agente en la nube ofrecen experiencias distintas. El modo de agente realiza las tareas en el IDE, lo que permite interactuar y obtener comentarios rápidamente. El agente en la nube está diseñado para trabajar de forma asíncrona como un miembro del equipo, mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás:

- Cómo explora tu proyecto el modo de agente de Copilot, identifica archivos pertinentes y coordina los cambios.
- Cómo implementa el modo de agente de GitHub Copilot funcionalidades en la interfaz de usuario y la capa de datos.
- Cómo revisar los cambios y las pruebas generados por el modo de agente de Copilot antes de combinarlos en el código base.

## Escenario

A medida que crece la lista de juegos, quieres permitir que los usuarios filtren por categoría y editor. En el ejercicio anterior ya añadiste una función auxiliar para los editores; ahora completarás con el modo de agente de Copilot el resto del trabajo de la capa de datos, la interfaz de usuario y las pruebas.

## Ejecuta el sitio de Tailspin Toys

Antes de hacer cambios, exploremos el sitio de Tailspin Toys para entender sus funciones actuales.

El sitio es una plataforma de financiación colectiva de juegos de mesa con temática de desarrollo. Permite mostrar una lista de juegos y ver sus detalles. Es una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle ORM en su frontmatter; no hay una API de backend independiente ni un framework de interfaz del lado del cliente. Las funciones reutilizables de acceso a datos se encuentran en `src/lib/`, y la interactividad se añade mediante un pequeño elemento `<script>` de Astro con ámbito limitado que utiliza las API estándar del DOM.

### Inicia el sitio

Se ha proporcionado la tarea `npm run dev` para iniciar el único servidor de desarrollo de Astro. Para ejecutarla en tu codespace de GitHub:

1. Vuelve a tu codespace. Deberías estar en `main` después del ejercicio anterior.
2. Abre una ventana de terminal nueva en el codespace con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea una rama nueva para el trabajo de filtrado y cámbiate a ella:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Ejecuta este comando para iniciar el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, debería aparecer un mensaje con la URL, parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir un navegador en `http://localhost:4321`, ciérralo seleccionando la **x**.

5. Abre el sitio manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> En un codespace, si seleccionas el enlace `localhost` en el terminal, se te redirigirá automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Se trata de un túnel privado al codespace, que ahora aloja el servidor web.

### Explora el sitio

Cuando el sitio esté en ejecución, podrás explorar sus funciones principales:

- **Página principal**: muestra una lista de juegos de mesa con títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, el editor y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve a tu codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. La selección de modo y modelo varía según el ejercicio; cada paso lo indica cuando es pertinente.

La primera versión del sitio funciona, pero queremos ampliarla con nuevas capacidades. Empecemos por revisar el backlog. Al crear el proyecto a partir de la plantilla, se generó automáticamente una lista de incidencias. Pide a GitHub Copilot que te las muestre.

1. Selecciona **Agent** en el menú desplegable de agentes de la vista Chat. El agente planifica e implementa cambios autónomamente en varios archivos, ejecuta comandos de terminal y usa herramientas.

   ![Selector de agente de la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores de este taller no muestran preferencia por un modelo concreto. Al preparar el taller usamos Claude Sonnet 4.5 y lo incluimos en las instrucciones para que las sugerencias de código sean relativamente uniformes y la experiencia sea más predecible. Sin embargo, como los modelos de lenguaje grandes son probabilísticos, es normal que las sugerencias que recibas difieran de las indicadas en el taller.

> [!NOTE]
> Debido a la naturaleza probabilística de los modelos de lenguaje grandes, Copilot podría usar un comando MCP distinto y aun así completar la tarea.

3. Envía a Copilot este prompt para pedirle que revise el backlog de incidencias:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para permitir que se ejecute el comando que enumera las incidencias.
5. Revisa la lista generada.

Observa que Copilot ha priorizado las incidencias según las que considera más útiles para el usuario.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, conviene revisar el archivo de instrucciones que proporcionará contexto a Copilot. Usarás el archivo de la [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funciones al sitio.

1. En tu codespace, ve a `.github/instructions/ui.instructions.md`.
2. Observa las directrices generales para añadir funciones, entre ellas:
   - Una descripción general de la arquitectura.
   - Principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - Enlaces a instrucciones específicas de varios tipos de archivo, como:
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> Los archivos de instrucciones pueden incluir referencias a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas a la ubicación del archivo de instrucciones. Esto permite reutilizar las instrucciones, dividir conjuntos complejos en partes más manejables e incluir ejemplos y plantillas.

## Implementa la funcionalidad de filtrado

Para completar el filtrado hay que hacer al menos tres cambios distintos en la aplicación:

- Añadir o mejorar la lógica de filtrado restante en la capa de datos (`src/lib/`).
- Añadir o actualizar las pruebas del comportamiento del filtrado.
- Actualizar la página de lista de juegos para incluir la interfaz de filtrado.

Además, las pruebas deben ejecutarse correctamente antes de combinar los cambios en el código base. El modo de agente de Copilot puede encargarse de estas tareas. Añadamos la funcionalidad.

1. Puedes continuar la conversación actual con Copilot o empezar otra seleccionando **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Ejemplo de selección del archivo de instrucciones de interfaz de usuario](../../../_images/ex3-select-instructions-file.png)

3. Comprueba que **Agent** sigue seleccionado en el menú desplegable de agentes de la vista Chat.

   ![Selector de agente de la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirma que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la funcionalidad según la incidencia relacionada del backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos relacionados con la funcionalidad. Debería encontrar las funciones de la capa de datos y la interfaz de usuario, además de las pruebas. Después, modificará los archivos y ejecutará las pruebas.

   ![Copilot explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas: explorar el proyecto, modificar archivos y ejecutar pruebas. El proceso puede tardar unos minutos según la complejidad de la tarea y del código base. Durante el trabajo, puede que aparezcan los botones **Keep** y **Undo** en el editor. Cuando Copilot termine, podrás aceptar o deshacer todos los cambios. No es necesario seleccionarlos mientras el trabajo siga en curso.

7. Cuando Copilot lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Cuadro de diálogo de Copilot Chat que pide permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede que haya pausas e incluso que algunas pruebas fallen durante el proceso. No pasa nada: Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea sin detectar errores.

   ![Sesión completa de Copilot Chat en el modo de agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado para detectar posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador donde se está ejecutando el sitio y explora la funcionalidad nueva.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en la ventana Copilot Chat.

## Resumen y próximos pasos

¡Enhorabuena! En este ejercicio has aprendido a usar el modo de agente de GitHub Copilot para añadir capacidades al sitio de Tailspin Toys. Has aprendido:

- Cómo implementa el modo de agente de GitHub Copilot funciones en la interfaz de usuario y la capa de datos.
- Cómo explora el proyecto, identifica archivos pertinentes y coordina los cambios.
- Cómo revisar los cambios y las pruebas generados por el modo de agente antes de combinarlos en el código base.

Ahora [probarás la funcionalidad con el servidor Playwright MCP][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio de exploración adicional: implementar la paginación

A medida que crece la lista de juegos, será necesario habilitar la paginación. Aplica lo aprendido en este ejercicio y pide a Copilot que actualice el sitio:

- Sigue las prácticas recomendadas existentes, incluido el uso de los archivos de instrucciones.
- Decide cómo quieres implementar la paginación: por ejemplo, si los usuarios podrán elegir el tamaño de página o si será fijo.
- Al redactar el prompt, proporciona a Copilot las directrices necesarias para que implemente la solución que necesitas.
- Puede que tengas que iterar con GitHub Copilot, pedir cambios y proporcionar contexto. Es lo normal al trabajar con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo se usan][choose-mode]
- [Modo de agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
