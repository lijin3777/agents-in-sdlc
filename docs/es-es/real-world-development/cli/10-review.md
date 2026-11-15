---
title: "Lección 10 - Repaso y próximos pasos"
description: "Repasa el flujo de trabajo de Copilot CLI, las dos solicitudes de incorporación de cambios, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has usado GitHub Copilot CLI a lo largo de un flujo de trabajo continuo en Tailspin Toys. En concreto:

- Preparaste un Codespace, instalaste Copilot CLI, exploraste el proyecto y encontraste la incidencia de filtrado.
- Añadiste las valoraciones por estrellas, revisaste el resultado en un navegador reenviado y combinaste manualmente tu primera solicitud de incorporación de cambios (PR).
- Partiste de la incidencia de filtrado, definiste el enfoque con el modo Plan, lo implementaste con Autopilot y lo revisaste en el modo Interactive.
- Guiaste al agente con instrucciones personalizadas, personalizaste la habilidad `quality-checks` existente y la usaste para ejecutar pruebas unitarias, lint y comprobaciones de tipos.
- Añadiste el servidor Playwright Model Context Protocol (MCP) y lo usaste para explorar el filtrado en un navegador real.
- Creaste y seleccionaste un agente personalizado de control de calidad (QA) para evaluar los requisitos, la cobertura, los resultados de la habilidad y las pruebas del navegador.
- Revisaste todo el cambio del filtrado y autorizaste Agent Merge para su PR.
- Exploraste comandos de barra para gestionar el contexto y los modelos, compartir sesiones y delegar tareas opcionalmente al agente en la nube.

## Lo que has publicado

El taller tiene dos hitos de PR, cada uno en una rama distinta creada a partir de `main` actualizado:

1. **Valoraciones por estrellas**: muestra en las tarjetas de juegos `starRating` y un estado explícito para los juegos sin valorar.
2. **Filtrado y flujo de trabajo de calidad**: implementa el filtrado, actualiza las instrucciones y las aplica a la funcionalidad, personaliza el informe de `quality-checks`, crea un perfil de QA e incluye las pruebas correspondientes.

Desde la planificación del filtrado hasta la apertura de su PR, has usado la misma conversación y rama. Hemos agrupado ese trabajo en una sola PR para simplificar el taller.

## Diferentes tipos de verificación

Has comprobado el código de varias maneras: con pruebas automatizadas, examinándolo tú mismo en el navegador y dejando que Copilot lo explore en el navegador mediante MCP. La habilidad `quality-checks` ha ejecutado pruebas unitarias, lint y comprobaciones de tipos, y ha incluido los resultados en el nuevo formato. El agente de QA ha reunido esos resultados con una revisión de los requisitos y de la cobertura de pruebas antes de la PR.

Las pruebas nuevas deben cubrir carencias reales; si una ejecución de QA no requiere añadir pruebas, el resultado puede ser correcto. Revisa el código y las pruebas antes de autorizar la combinación, y vuelve a recopilar las pruebas afectadas después de hacer cambios.

## Prácticas recomendadas

El contexto y las herramientas que proporcionas influyen en el trabajo de Copilot. En este taller has actualizado las instrucciones, personalizado una habilidad, creado un perfil de QA y configurado un servidor MCP. Reutiliza estas personalizaciones en otras conversaciones y adáptalas a las necesidades del equipo. Las instrucciones definen estándares; las habilidades describen tareas repetibles; los agentes personalizados definen roles especializados; y los servidores MCP conectan herramientas externas. Revisa los cambios reales y los resultados de las herramientas, no solo el resumen del agente.

Elige el **modo y el modelo** adecuados para la tarea. Usa **Plan** para definir un enfoque antes de empezar, **Interactive** para seguir participando en cambios concretos y **Autopilot** para tareas bien acotadas. Elige un modelo más rápido para ediciones rutinarias y uno más capaz para tareas complejas.

El contexto sigue siendo tan importante como la infraestructura. Describir claramente *qué* quieres crear, *por qué* y *cómo* influye de forma considerable en el resultado.

## Otras funciones

Ya has completado el flujo de trabajo básico. Estas son algunas funciones más de la CLI que puedes explorar:

- `/review` para pedir al agente de revisión de código que analice los cambios.
- `/rubber-duck` para explicar un problema y obtener otra perspectiva.
- `/fleet` para coordinar tareas independientes en paralelo.
- `/worktree` para aislar una tarea distinta.
- `/delegate` para enviar una tarea al agente de Copilot en la nube.

## Próximos pasos

La mejor forma de mejorar con cualquier herramienta es seguir usándola. Úsala con código de producción, en proyectos personales o para crear esa pequeña aplicación que llevas años imaginando. Comparte lo aprendido con el equipo y aprovecha lo que aprendan los demás. Y, como siempre, explora la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Incorporar Foundry][foundry]. Para comparar otros entornos, explora los talleres de [VS Code][vscode], la [aplicación GitHub Copilot][app] o el [agente de Copilot en la nube][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
