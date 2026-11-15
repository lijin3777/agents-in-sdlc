---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los dos ejercicios anteriores has pedido al agente de Copilot en la nube que realice tres tareas distintas para mejorar la experiencia de usuario y añadir funcionalidades. Aunque el agente trabaja de forma autónoma y asíncrona, es importante poder supervisar estas tareas.

Dispones de varias herramientas para gestionar las tareas asignadas al agente en la nube, como la [página de agentes][agents-page] de GitHub.com. Desde este panel de control puedes ver todas las tareas del agente que tienen solicitudes de incorporación de cambios abiertas. También puedes explorar las operaciones realizadas y orientar las sesiones en curso.

En esta lección:

- Explora la página de agentes para supervisar las tareas del agente en la nube.
- Guía una sesión en curso para pedir una funcionalidad adicional.

## Escenario

Después de asignar al agente la tarea de crear un modo de alto contraste, el equipo se dio cuenta de que también sería un buen momento para añadir un modo claro. Como ya se estaba actualizando el estilo del sitio y añadiendo un control para cambiar de modo, parecía razonable incluir también esta funcionalidad. Quieres orientar el trabajo del agente para que añada el modo claro además del modo de alto contraste.

## Revisa las tareas del agente de Copilot en la nube

Veamos el estado actual de las tareas que se han asignado al agente de Copilot en la nube.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas en el panel principal y en el panel izquierdo. Deberías ver las tareas que has asignado a Copilot, entre ellas:
   - Actualizar la documentación del código.
   - Generar API para modificar juegos.
   - Añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en curso. Revisa las operaciones que ha realizado Copilot, por ejemplo:
   - Obtener el código del repositorio.
   - Crear el entorno de trabajo de Copilot.
   - Configurar servidores MCP.
   - Realizar distintos pasos para completar la tarea asignada.

> [!NOTE]
> Los pasos concretos de la lista variarán según el estado del trabajo de Copilot y el enfoque que haya adoptado.

4. Observa también el panel de solicitudes de incorporación de cambios que aparece a la derecha. Desde ahí puedes ver la PR y los archivos modificados para supervisar el trabajo.

## Guía al agente en la nube

Ahora que has visto las tareas activas, pidamos a Copilot que añada el control del modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título concreto variará según el nombre que haya elegido Copilot y el estado actual del trabajo.

   ![Sesión de accesibilidad en el panel de control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos hasta que indique que ha terminado la configuración y ha empezado a trabajar. Lo notarás cuando aparezcan mensajes parecidos a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe el siguiente prompt:

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Tarea del agente en la nube con el cuadro de diálogo para guiar la sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot confirma que lo ha recibido y lo incorpora a su flujo de trabajo.

## Deja que Copilot trabaje

Como antes, Copilot empezará a trabajar en la tarea actualizada. Incorporará la nueva solicitud al flujo cuando termine el paso en el que estaba trabajando al recibir el mensaje.

El proceso tardará varios minutos. Es un buen momento para hacer una pausa y repasar lo que has aprendido y explorado hasta ahora.

## Resumen y próximos pasos

En esta lección has explorado la página de agentes de Copilot, el centro desde el que puedes supervisar y orientar las tareas del agente de Copilot en la nube. Este panel te permite hacer un seguimiento de las tareas activas y completadas, revisar el trabajo en curso y redirigir las sesiones para ajustar su alcance o proporcionar más indicaciones.

Has explorado estos conceptos:

- exploraste el panel de control y la página de agentes para supervisar las tareas del agente en la nube.
- redirigiste una sesión en curso para solicitar una funcionalidad adicional.

Mientras Copilot termina de trabajar en las funciones de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
