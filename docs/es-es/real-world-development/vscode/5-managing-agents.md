---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando usas el modo de agente de GitHub Copilot, trabaja de forma autónoma: explora el código base, propone cambios, edita archivos y ejecuta comandos. Como trabaja en tiempo real en tu equipo, Copilot Chat de VS Code muestra cada llamada a herramienta y cada edición de archivo a medida que se producen. Puedes revisar cada diferencia en línea, aceptar o rechazar los cambios y guiar la conversación con prompts de seguimiento para ajustar o ampliar el trabajo sin salir del editor.

En este ejercicio:

- Supervisarás en tiempo real el trabajo del agente de accesibilidad.
- Revisarás y aceptarás las ediciones propuestas en los archivos.
- Guiarás al agente durante la sesión para que añada un control del modo claro además del modo de alto contraste.

## Escenario

El agente personalizado de accesibilidad que has usado en el [Ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño ha solicitado además un control del modo claro para quienes consideran que el tema predeterminado es demasiado oscuro. En vez de iniciar una conversación nueva, guiarás la sesión actual para ampliar el trabajo.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada edición aparece en línea en el chat con las diferencias y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del Ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para ver cada llamada a herramienta. Verás lecturas de archivos, ediciones y los comandos de terminal que solicita ejecutar.
3. Cuando el agente proponga editar un archivo, las diferencias aparecerán en línea:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para deshacerlo y permitir que el agente vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed** de la parte superior del chat para ver todos los archivos modificados durante la sesión.

> [!NOTE]
> Si una llamada a herramienta requiere permiso (por ejemplo, para ejecutar un comando de terminal), el agente se detendrá y mostrará un aviso **Allow**. Aprueba solo los comandos que consideres seguros.

5. Espera a que el agente termine de implementar el modo de alto contraste. Deberías ver un mensaje final que resuma los cambios.

## Guía al agente para añadir el modo claro

Ya está implementado el modo de alto contraste. Ahora ampliarás la misma conversación para añadir un control del modo claro. Como continuarás en la misma sesión, el agente conservará todo el contexto del trabajo anterior, incluidos los componentes modificados, el enfoque de estilo y el método usado para guardar los datos en el almacenamiento local.

1. Envía el siguiente prompt de seguimiento desde el campo de texto de Copilot Chat:

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. El agente continuará desde donde lo dejó, explorará los cambios realizados y propondrá las modificaciones necesarias para el modo claro.
3. Revisa cada cambio nuevo cuando aparezca, como hiciste con el modo de alto contraste. Usa **Keep** o **Undo** en cada diferencia.
4. Si el agente sigue un enfoque que no quieres —por ejemplo, intenta reorganizar los controles de una manera que no te convence—, puedes detenerlo con el botón **Stop** junto al campo de texto y enviarle otro mensaje para aclararlo.

> [!TIP]
> Guiar una sesión en curso es más rápido que empezar de nuevo. Usa prompts de seguimiento para solicitar ajustes («simplifica el CSS y usa variables CSS en lugar de duplicar selectores»), corregir el enfoque («no añadas un tercer control; integra el modo claro en el panel de configuración existente») o ampliar el alcance («añade también métodos abreviados de teclado para ambos controles»).

## Revisa el conjunto de trabajo

Antes de confirmar los cambios, revisa todo lo que ha modificado el agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa todos los archivos modificados. Deberías ver cambios en los componentes Astro, los estilos y las pruebas relacionadas.
3. Abre algunos archivos modificados y revisa las diferencias. Confirma que la implementación sigue las pautas de accesibilidad del agente personalizado: atributos ARIA, navegación con el teclado, HTML semántico y persistencia en el almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en la [siguiente lección][next-lesson].

## Resumen y próximos pasos

Has usado el modo de agente de Copilot Chat para supervisar y guiar una sesión en curso: has revisado las ediciones cuando aparecían, aceptado o rechazado cambios y enviado prompts de seguimiento para ampliar el trabajo sin perder el contexto.

En esta lección:

- Supervisaste el trabajo del agente en tiempo real.
- Aceptaste y rechazaste ediciones individuales.
- Guiaste una sesión en curso con prompts de seguimiento.

En la [siguiente lección][next-lesson] publicarás el trabajo de accesibilidad en una solicitud de incorporación de cambios y revisarás los cambios generados por el agente personalizado local durante el taller.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Usar el modo de agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
