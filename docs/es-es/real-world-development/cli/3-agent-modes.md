---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Usa Plan para acordar un enfoque, Autopilot para crear el filtrado a partir de una incidencia e Interactive para revisar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Hemos empezado por añadir una pequeña funcionalidad al proyecto. Sin embargo, los cambios de mayor alcance requieren un proceso más sólido. Por suerte, GitHub Copilot CLI está diseñado para trabajar con el flujo existente de una organización y ayudarnos a desarrollar lo adecuado de la manera correcta. En esta primera de varias lecciones seguirás un proceso habitual de desarrollo con agentes: partirás de una incidencia para crear una funcionalidad, comprobarás que el código es válido y que la funcionalidad se comporta como esperas y, finalmente, la integrarás correctamente en el proyecto.

> [!NOTE]
> Seguirás usando la misma conversación y rama durante el resto del flujo de trabajo de la funcionalidad. Normalmente se usarían ramas o PR distintas para los diferentes tipos de archivos, pero en este taller simplificaremos el proceso para centrarnos en los conceptos principales.

En esta lección:

- Inicia una conversación nueva de Copilot a partir de una incidencia de GitHub.
- Define los requisitos en el modo Plan.
- Implementa la funcionalidad con el modo Autopilot.
- Revisa el código.
- Valida manualmente la funcionalidad en un navegador reenviado.

Mientras completas esta funcionalidad, actualizarás las instrucciones del repositorio, personalizarás la habilidad `quality-checks` existente, añadirás validación con MCP, crearás un agente de QA y abrirás la PR de la funcionalidad.

## Escenario

El catálogo de Tailspin Toys crece y los visitantes necesitan filtrar los juegos por categoría y editor. La incidencia del backlog describe la funcionalidad, pero es necesario acordar algunos detalles antes de programar, como la combinación de categorías. Usarás el modo Plan para resolver esas decisiones y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

Incorporar agentes de programación con IA a tu flujo de desarrollo no cambia los fundamentos. ¡De hecho, los hace aún más importantes! La mayoría de los desarrolladores siguen un proceso parecido a este:

1. Partir de una incidencia registrada que describa el trabajo pendiente.
2. Crear un plan de lo que hay que desarrollar.
3. Crear y revisar el código.
4. Ejecutar las pruebas para validar el código.
5. Validar manualmente la funcionalidad nueva.
6. Crear una solicitud de incorporación de cambios (PR).
7. Una vez revisado el código y completada correctamente la integración continua, combinarlo.

> [!NOTE]
> Los detalles concretos dependen del equipo y de la organización, pero la mayoría de los procesos siguen alguna variante del esquema anterior.

Seguir este enfoque habitual permite comprobar que el código generado por IA cumple los requisitos y pasa por las mismas revisiones que el código escrito manualmente.

## Modos de conversación

El **modo de conversación** controla el grado de autonomía del agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre los modos:

- **Interactive**: trabajas con el agente. Este propone cambios y espera tus indicaciones antes de continuar.
- **Plan**: el agente crea primero un plan y no puede editar los archivos del proyecto.
- **Autopilot**: el agente trabaja de forma autónoma: escribe código, ejecuta pruebas y repite el proceso hasta completar la tarea.

Empieza en el modo Plan, revisa el plan y, después, usa Autopilot para implementarlo.

## Parte de la incidencia

Antes de empezar a trabajar en el filtrado, vuelve a tu Codespace y asegúrate de que el repositorio y el terminal están listos.

1. Vuelve a tu Codespace. Si está detenido, reinícialo antes de continuar.
2. Confirma que se ha combinado la PR de valoración por estrellas.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa <kbd>Tab</kbd> dos veces para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Usa las teclas de dirección para resaltar la incidencia titulada **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia al prompt y volver a la pestaña **Session**.

Observa que el prompt ahora empieza por `#7` (o por un número parecido). El carácter `#` permite incluir en el contexto una incidencia o solicitud de incorporación de cambios de GitHub.

## Planifica la funcionalidad de filtrado

La planificación permite definir cómo se implementará una funcionalidad o se realizará una tarea antes de encargársela a Copilot. Siempre es buena idea dedicar algo de tiempo a planificar las tareas complejas. Cambia al modo Plan y pide a Copilot que cree el plan.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Plan. Confirma que el indicador debajo del prompt muestra **Plan**.
2. Después de la referencia a la incidencia que has añadido en el paso anterior, escribe el siguiente prompt:

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot empezará a explorar el proyecto y a determinar el mejor enfoque para elaborar el plan.

3. Puede que Copilot te haga preguntas sobre cómo debe funcionar el filtrado. Respóndelas según tus preferencias; no hay respuestas incorrectas.
4. Cuando el plan esté listo, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandirlo.
5. Desplázate por el plan para revisarlo.
6. Pide a Copilot que modifique cualquier parte del plan que no se ajuste a tus decisiones.

## Aprueba Autopilot

Con el plan ya escrito y revisado, es hora de implementarlo. Deja que Copilot trabaje con el modo Autopilot.

Autopilot permite que Copilot itere hasta considerar que el problema está resuelto.

1. Selecciona **Accept plan and build on autopilot (recommended)** o la opción equivalente de tu versión.
2. Confirma que el indicador de modo debajo del prompt muestra **Autopilot**.
3. Observa cómo Copilot sigue el plan acordado, genera código y ejecuta pruebas.

> [!NOTE]
> La aprobación puede iniciar la implementación de inmediato, así que revisa el plan antes. Si Copilot informa de dependencias ausentes o de un conflicto de puertos, resuelve el problema de configuración antes de dar las comprobaciones por completadas.

## Revisa y comprueba la implementación

Como con cualquier otro código, debes revisar el código generado antes de combinarlo. Revisemos el código y ejecutemos el sitio para comprobar que todo funciona correctamente.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar en el modo Interactive. Confirma que el indicador de modo ya no muestra **Plan** ni **Autopilot**.
2. Escribe `/diff` e inspecciona la implementación del filtrado y las pruebas.
3. Compara el resultado con la incidencia y las decisiones que has tomado durante la planificación.
4. Cuando termines la revisión, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.
5. Revisa los resultados de las comprobaciones del proyecto y pide a Copilot que corrija los errores que haya.

## Explora la funcionalidad nueva

El código parece correcto, pero ¿se ejecuta? Iniciemos la aplicación como antes y abramos el sitio desde el puerto reenviado de Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba los filtros por categoría y editor, así como las combinaciones que acordaste durante la planificación.
4. Confirma que las opciones para restablecer los filtros y mostrar resultados vacíos coinciden con la incidencia y tus decisiones.
5. Vuelve a tu Codespace y pide a Copilot que detenga el servidor de desarrollo que ha iniciado.

## Resumen y próximos pasos

Has usado distintos modos de conversación para crear y revisar una funcionalidad. En esta lección:

- Iniciaste una conversación nueva de Copilot a partir de una incidencia de GitHub.
- Definiste los requisitos en el modo Plan.
- Implementaste la funcionalidad con el modo Autopilot.
- Revisaste el código.
- Validaste manualmente la funcionalidad en un navegador reenviado.

A continuación, [profundizarás en cómo se genera el código y cómo seguir las prácticas documentadas mediante instrucciones personalizadas][next-lesson].

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
