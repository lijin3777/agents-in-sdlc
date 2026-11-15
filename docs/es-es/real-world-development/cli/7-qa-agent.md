---
title: "Lección 7 - Crea y utiliza un agente de QA"
description: "Crea un agente personalizado de QA que combine los requisitos de la incidencia, la habilidad quality-checks y Playwright MCP."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Has usado la habilidad `quality-checks` para ejecutar comprobaciones automatizadas y Playwright MCP para observar la experiencia de filtrado en un navegador. Ahora combinarás estas capacidades en un agente personalizado con un proceso de QA bien definido.

En esta lección:

- Explora cómo trabaja un agente personalizado con instrucciones, habilidades y herramientas MCP.
- Crea e inspecciona un perfil de control de calidad reutilizable.
- Selecciona el agente de QA y revisa sus conclusiones respecto a la incidencia de filtrado.

## Escenario

Antes de abrir una solicitud de incorporación de cambios (PR), Tailspin Toys quiere revisar de forma uniforme los requisitos, la calidad del código, las comprobaciones automatizadas, la cobertura de pruebas y el comportamiento en el navegador. Un agente personalizado puede coordinar el proceso de QA y generar un informe reutilizable.

## ¿Qué es un agente personalizado?

Un agente personalizado es una versión especializada de Copilot definida en un perfil de Markdown. El perfil describe el propósito, las instrucciones y las herramientas disponibles para el agente. En este taller definirás el rol de QA en `.github/agents/qa.agent.md` y lo seleccionarás en Copilot CLI.

Las personalizaciones que has usado tienen funciones distintas. Las instrucciones del repositorio describen los estándares del equipo. La habilidad `quality-checks` agrupa comprobaciones repetibles. Playwright MCP proporciona herramientas de navegador. El perfil de QA indica a Copilot cómo utilizar esas capacidades para evaluar los requisitos e informar de los resultados. No sustituye a las otras personalizaciones ni requiere otra conversación.

## Crea el perfil de QA

Antes de abrir la PR de la funcionalidad, pedirás a Copilot que cree un perfil de QA reutilizable. El perfil definirá tanto las comprobaciones de QA como los límites que debe respetar.

1. Vuelve a tu Codespace y confirma que la conversación de filtrado está en el modo Interactive.
2. Pide al agente predeterminado que cree el agente personalizado:

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## Inspecciona el perfil

Antes de usar el nuevo agente, revisa su perfil para confirmar que Copilot ha incorporado el proceso de QA y los límites previstos. Así evitarás que un perfil incompleto o demasiado amplio modifique la funcionalidad cuando solo querías verificarla.

1. Escribe `/diff` y abre `.github/agents/qa.agent.md`.
2. Lee el frontmatter. `description` es obligatorio; `name` es opcional, pero incluirlo da al agente un nombre claro en la interfaz.
3. Lee las instrucciones del perfil y confirma que QA parte de los requisitos, sigue las instrucciones del repositorio, ejecuta la habilidad `quality-checks` y usa Playwright MCP.
4. Confirma que QA aporta pruebas que respaldan sus conclusiones, pide permiso antes de modificar el código de implementación y no confirma cambios ni abre solicitudes de incorporación de cambios.
5. Si al perfil generado le falta alguna de estas responsabilidades o restricciones, pide al agente predeterminado que lo revise antes de continuar.

## Ejecuta el control de QA respecto a la incidencia

Copilot CLI carga los agentes del proyecto al iniciar una conversación. Tras crear el perfil, reanuda la misma conversación de filtrado y selecciona QA para que pueda usar la incidencia y las decisiones de planificación que ya están en el contexto.

1. Activa el agente con este prompt:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como acabas de crear el agente, es posible que no aparezca en la lista de agentes disponibles. Aunque no aparezca en la lista, el comando anterior lo activará.

2. Usa el siguiente prompt para pedir al agente de QA que revise la funcionalidad:

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. El agente de QA iniciará la revisión.
4. Lee el informe cuando termine.

## Resumen y próximos pasos

Has añadido un rol especializado reutilizable y revisado su trabajo. En esta lección:

- Exploraste cómo trabaja un agente personalizado con instrucciones, habilidades y herramientas MCP.
- Creaste e inspeccionaste un perfil de control de calidad reutilizable.
- Seleccionaste el agente de QA y revisaste sus conclusiones respecto a la incidencia de filtrado.

Ya tienes la implementación, la actualización de la habilidad, el perfil de QA, las pruebas y el informe de verificación listos para revisar. A continuación, [los reunirás en una PR de funcionalidad y usarás Agent Merge][next-lesson].

## Recursos

- [Crear agentes personalizados para Copilot CLI][create-agents]
- [Configuración de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
