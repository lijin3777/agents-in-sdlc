---
title: "Lección 8 - Crear y combinar la PR de la funcionalidad"
description: "Revisa conjuntamente el filtrado, las instrucciones, la actualización de la habilidad, el perfil de QA y las pruebas; después, crea una PR y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La implementación del filtrado, las actualizaciones de instrucciones y de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una sola rama. Es hora de revisarlos juntos y abrir una solicitud de incorporación de cambios. Ya has combinado personalmente la solicitud de incorporación de cambios (PR) de valoración por estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las actualizaciones de instrucciones y de la habilidad y el agente de QA en varias PR. Para agilizar el taller, has mantenido todo el flujo de filtrado y calidad en una sesión y una rama, y todo ese trabajo se incluirá en esta PR.

En esta lección:

- Aprende qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación.
- Inspecciona la PR completa de la funcionalidad y las pruebas de verificación.
- Autoriza Agent Merge solo después de revisarla y confirma que se ha combinado.

## Escenario

Durante el flujo de trabajo del filtrado, has usado Copilot para planificar, implementar y verificar una funcionalidad. Ahora Tailspin Toys quiere automatizar el trabajo restante de la PR, pero mantener bajo el control del desarrollador la autorización para combinarla.

## Introducción a Agent Merge

**Agent Merge** automatiza el trabajo restante para integrar una solicitud de incorporación de cambios en la aplicación GitHub Copilot. Al activarlo, la sesión de la aplicación lee la PR y resuelve los bloqueos: corrige las comprobaciones de integración continua (CI) fallidas, responde a comentarios de revisión y vuelve a basar la rama cuando es necesario. La combina en cuanto GitHub lo permite. Se ejecuta en segundo plano, sigue funcionando aunque se reinicie la aplicación y se desactiva cuando se combina la solicitud.

Hasta ahora has seleccionado **Merge pull request** manualmente. Agent Merge puede encargarse de ello, pero para editar código y combinar todavía necesita tu autorización explícita. Revisa las acciones permitidas y el trabajo antes de conceder permiso para combinar.

## Usa Agent Merge para gestionar la PR

Ya has creado y revisado todo el código. Deja que Agent Merge gestione el proceso de la PR.

1. Usa el selector de agentes para seleccionar **Default agent**.
2. Selecciona el menú desplegable junto a **Create PR**.
3. Selecciona **Agent Merge**. El botón cambiará a **Agent Merge**.
4. Selecciona el botón **Agent Merge** para iniciar el proceso. Seleccionar la opción del menú desplegable no lo inicia por sí solo.

Se iniciará el proceso de Agent Merge. El agente:

- Creará la solicitud de incorporación de cambios con un título y una descripción.
- Si has iniciado la sesión desde una incidencia, la mencionará en el cuerpo de la descripción.
- Volverá a basar la rama o gestionará posibles conflictos con la rama de destino.
- Supervisará el proceso de CI para asegurarse de que se superen todas las comprobaciones.
- Supervisará los comentarios de otros desarrolladores y de la revisión de código de Copilot en la PR, y actualizará el código para resolverlos.
- Puede combinar automáticamente la PR cuando todo termine correctamente.

Permitamos que Agent Merge combine también la PR cuando se superen todas las comprobaciones.

5. Revisa las diferencias de la PR y las pruebas de verificación antes de conceder permiso para combinar.
6. Selecciona el menú desplegable junto a **Agent Merge**.
7. Comprueba que **Merge pull request** está marcado.
8. Pide explícitamente al agente que combine la PR:

   ```plaintext
   Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
   ```

9. Supervisa el progreso y resuelve los bloqueos que se indiquen. Antes de continuar, confirma que el estado de la PR es **Merged**; habilitar el permiso de combinación o tener un agente en ejecución no confirma que se haya combinado.

> [!IMPORTANT]
> Agent Merge no omite las protecciones del repositorio ni los permisos que falten. Resuelve esos bloqueos antes de continuar.

## Resumen y próximos pasos

Has automatizado varias partes del proceso de desarrollo: generar código, probarlo y validarlo, y ahora gestionar las solicitudes de incorporación de cambios. En esta lección:

- Aprendiste qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación.
- Inspeccionaste la PR completa de la funcionalidad y las pruebas de verificación.
- Autorizaste Agent Merge después de la revisión y confirmaste que se ha combinado la PR.

A continuación, [usarás un lienzo existente y crearás uno de clasificación][next-lesson] para explorar una forma más completa de examinar, planificar y visualizar el trabajo con el agente.

## Recursos

- [Gestionar incidencias y solicitudes de incorporación de cambios con la aplicación GitHub Copilot][managing-issues-prs]
- [Acerca de la aplicación GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
