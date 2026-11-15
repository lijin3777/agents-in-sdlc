---
title: "Ejercicio 2 - Agente de GitHub Copilot en la nube"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que muy pocas organizaciones, si es que hay alguna, no tengan problemas con la deuda técnica. Puede tratarse de incidencias de seguridad pendientes, código heredado que necesita actualizarse o solicitudes de funcionalidad que llevan tiempo en el backlog por falta de recursos. El agente de Copilot en la nube está diseñado para realizar tareas como actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, genera una solicitud de incorporación de cambios en borrador para que la revise un desarrollador. Así puedes delegar tareas tediosas, acelerar el desarrollo y dedicarte a cuestiones de mayor alcance.

Con el agente de Copilot en la nube explorarás:

- cómo personalizar el entorno en el que se genera el código.
- cómo asegurarte de que las operaciones se realicen de forma segura.
- la importancia de definir incidencias con un alcance claro.
- cómo asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica que quiere resolver. Los contratistas que crearon la primera versión del sitio dejaron la documentación en mal estado: comprobarás que no hay ninguna. Como primer paso, quieren añadir comentarios TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño está listo para mejorar cómo se descubren los juegos. Quiere mostrar en la página de detalles de cada juego otros títulos de la misma categoría, para que los visitantes sigan explorando. No necesita una implementación refinada; solo una funcionalidad que permita realizar pruebas de aceptación de la experiencia de usuario. Aunque esto está bloqueando el trabajo, hay otras incidencias de mayor prioridad.

Estos son ejemplos de tareas que pueden quedar relegadas fácilmente y que se pueden asignar al agente de Copilot en la nube. El agente puede trabajar en ellas de forma asíncrona mientras el desarrollador se ocupa de otras tareas, y después puede volver a revisar el trabajo de Copilot y confirmar que todo está como esperaba.

## Introducción al agente de GitHub Copilot en la nube

[El agente de GitHub Copilot en la nube](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano de forma parecida a un desarrollador. Al igual que ocurre al trabajar con una persona, puedes asignarle trabajo de distintas maneras, por ejemplo, [asignándole una incidencia de GitHub](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Cuando se le asigna una incidencia, Copilot crea una solicitud de incorporación de cambios en borrador para hacer un seguimiento del trabajo, configura un entorno y empieza a trabajar. Puedes consultar la sesión de Copilot mientras está en curso o una vez terminada. Cuando la solución esté lista para que la revises, Copilot te mencionará en la solicitud.

## La importancia de definir bien el alcance

GitHub Copilot no hace magia: no puedes saltarte el análisis. Incluso las tareas que parecen sencillas se complican al examinarlas en detalle. Por eso, [define cuidadosamente el alcance de las tareas que asignes al agente de Copilot en la nube][cloud-agent-best-practices]. Considéralo un programador colaborativo basado en IA: trabaja por etapas, aprende, experimenta y adapta el proceso. Los fundamentos del desarrollo de software no cambian con la IA generativa.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores has conocido las instrucciones personalizadas y cómo guían a Copilot. Si ya has completado un ejercicio práctico sobre instrucciones personalizadas, habrás visto cómo funciona `.github/copilot-instructions.md`. Si no, aprovecha para echarle un vistazo. Antes de asignar trabajo al agente de Copilot en la nube, revisa los archivos de instrucciones que ya incluye este repositorio para que después puedas observar su efecto.

Abre los siguientes archivos en la interfaz web de GitHub o en un codespace que ya tengas en ejecución:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, especialmente las expectativas para las convenciones de TypeScript y los comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observa el frontmatter `applyTo`, que limita estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes al agente en la nube la incidencia `Code lacks documentation` en la sección siguiente, observa si la solicitud de incorporación de cambios incluye comentarios TSDoc, sigue las convenciones de TypeScript y contiene comentarios de cabecera. Estas instrucciones provienen de los archivos que acabas de revisar. Volveremos a señalarlo cuando revisemos las solicitudes en un ejercicio posterior.

## Configura el entorno de desarrollo del agente de Copilot en la nube

Normalmente, para crear código hay que preparar un entorno concreto y ejecutar scripts de configuración para asegurarse de que está listo. Lo mismo ocurre al asignar tareas a Copilot, que trabaja de forma parecida a un ingeniero de software.

El agente en la nube usa [GitHub Actions][github-actions] como entorno de trabajo. Puedes personalizarlo creando un [flujo de trabajo de configuración especial][setup-workflow] en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que el agente empiece a trabajar. Así puede acceder a las herramientas y dependencias necesarias. Este flujo ya está configurado para facilitar el taller y permitirte explorar esta función. Instala Node.js, las dependencias del proyecto y los binarios del navegador para las pruebas de extremo a extremo, y configura la base de datos SQLite migrada y con datos iniciales de la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña Actions del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Usa los permisos mínimos necesarios para tus pasos. Copilot recibe su propio token para sus operaciones.
    permissions:
      # Si quieres clonar el repositorio como parte de los pasos de configuración, por ejemplo, para instalar dependencias, necesitas el permiso contents: read. Si no clonas el repositorio, Copilot lo hará automáticamente cuando terminen los pasos.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración de la interfaz y la aplicación: Node.js (la aplicación completa ahora usa Astro, Drizzle y libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y carga la base de datos SQLite local para que las compilaciones y pruebas dispongan de datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro flujo de trabajo de GitHub, pero tiene algunos aspectos importantes:

- Incluye un único trabajo llamado `copilot-setup-steps`. Este trabajo se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la solicitud de incorporación de cambios.
- Observa el desencadenador `workflow_dispatch`, que permite ejecutar el flujo de trabajo manualmente desde la pestaña **Actions** del repositorio. Así puedes comprobar que funciona correctamente sin esperar a que Copilot lo ejecute.

## Añade documentación

Aunque todo el mundo reconoce la importancia de la documentación, muchos proyectos tienen información desactualizada o carecen de ella. Este tipo de deuda técnica suele quedar sin resolver, reduce la productividad y dificulta el mantenimiento del código o la incorporación de nuevos desarrolladores. Afortunadamente, Copilot destaca a la hora de crear documentación, así que esta es una incidencia ideal para asignar al agente en la nube. Trabajará en segundo plano para generar la documentación necesaria. En un ejercicio futuro volverás para revisar el resultado.

1. Abre una pestaña nueva del navegador y ve a tu repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de nuevas incidencias.
4. Selecciona **Blank issue** para crear una incidencia en blanco.
5. Escribe `Code lacks documentation` en **Title**.
6. Escribe lo siguiente en **Description**:

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

   ![Asignación de una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

   ![Detalles de la asignación a Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecciona la pestaña **Pull Requests**.
11. Abre la solicitud de incorporación de cambios (PR) que se acaba de generar. Su título será parecido a `[WIP]: Code lacks documentation`. Si todavía no aparece ninguna PR, espera un momento y actualiza la ventana del navegador.
12. Al cabo de unos minutos, Copilot debería haber creado una lista de tareas.

> [!NOTE]
> La lista de tareas puede tardar varios minutos en aparecer en la PR. Copilot está preparando su entorno (ejecutando el flujo de trabajo descrito anteriormente), analizando el proyecto y determinando cómo abordar la tarea.

13. Revisa la lista y las tareas que va a completar.
14. Desplázate por la cronología de la solicitud de incorporación de cambios. Deberías ver una actualización que indica que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

   ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la ventana para ver el indicador actualizado.

16. Observa que puedes desplazarte por la sesión en directo y ver cómo Copilot resuelve el problema. Esto incluye explorar el código, entender el estado del proyecto, detenerse a analizar y elegir el plan adecuado, y crear código.

El proceso tardará varios minutos. Uno de los objetivos principales del agente de Copilot en la nube es permitirle trabajar de forma asíncrona y dejarte tiempo para ocuparte de otras tareas. Aprovecharemos esa función: asignaremos otra tarea al agente y, mientras trabaja, nos centraremos en desarrollar una funcionalidad para la aplicación.

## Añade una sección de juegos relacionados a la página de detalles

Como hemos visto, una de las grandes ventajas del agente de Copilot en la nube es que permite dividir el trabajo: puedes centrarte en un conjunto de tareas mientras él se ocupa de otro. Añadir una sección de juegos relacionados para el equipo de diseño quizá no lleve mucho tiempo, pero podrías dedicar ese tiempo a otras tareas. ¡Asignemos el trabajo al agente en la nube!

1. Vuelve a tu repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de nuevas incidencias.
4. Selecciona **Blank issue** para usar la plantilla en blanco.
5. Escribe `Show related games on the game details page` en **Title**.
6. Escribe lo siguiente en **Description**:

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

   ![Asignación de una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Poco después, deberías ver unos 👀 en el primer comentario de la incidencia que indican que Copilot está trabajando en ella.

![Copilot indica con el emoji de ojos que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en tu segunda solicitud. El agente en la nube funciona de manera parecida a un ingeniero de software: no necesitas supervisarlo continuamente, puedes revisar el resultado cuando termine. Ahora vamos a crear y usar agentes personalizados.

## Resumen y próximos pasos

En esta lección has explorado el [agente de GitHub Copilot en la nube][copilot-agents]. Puedes asignar incidencias a Copilot para que las resuelva de forma asíncrona. Puedes usarlo para abordar deuda técnica, crear funcionalidades o migrar código de un framework a otro.

Has explorado estos conceptos:

- cómo personalizar el entorno en el que se genera el código.
- cómo asegurarte de que las operaciones se realicen de forma segura.
- la importancia de definir incidencias con un alcance claro.
- cómo asignar incidencias a Copilot.

Mientras el agente en la nube trabaja, puedes centrarte en crear y utilizar agentes personalizados. [El agente de Copilot en la nube también puede usar servidores MCP][cloud-agent-mcp] y puede acceder a las instrucciones personalizadas que has explorado en módulos anteriores.

## Recursos

- [Acerca del agente de Copilot en la nube][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de trabajo de configuración del agente en la nube][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
