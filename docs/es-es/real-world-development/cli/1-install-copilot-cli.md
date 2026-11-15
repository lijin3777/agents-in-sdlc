---
title: "Lección 1 - Instalar GitHub Copilot CLI"
description: "Instala y autentica Copilot CLI en tu Codespace, familiarízate con el entorno y localiza la incidencia de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] es un potente asistente de programación con agentes que se ejecuta en el terminal. Te permite explorar bases de código, generar código, ejecutar comandos e interactuar con herramientas externas desde la línea de comandos. Puedes delegar tareas, solicitar cambios y mantener la concentración. El primer paso, como imaginarás, es instalar la herramienta. Por suerte, puedes hacerlo con herramientas que ya conoces.

En esta lección:

- Instala GitHub Copilot CLI con npm.
- Autentícate con tu cuenta de GitHub.
- Confía en el repositorio del taller y prueba una conversación breve.
- Busca la incidencia de filtrado mediante el servidor GitHub MCP integrado.

## Escenario

Tu equipo está empezando a usar agentes de IA para gestionar un backlog cada vez mayor. Copilot CLI lleva esa capacidad a la terminal, donde muchos desarrolladores ya trabajan habitualmente. En esta lección lo instalarás, autenticarás y dejarás listo para usarlo durante el resto del taller.

## Instala Copilot CLI

Puedes instalar Copilot CLI mediante [npm][install-cli], WinGet y Homebrew. Como GitHub Codespaces incluye Node.js preinstalado, usarás npm.

1. Vuelve a tu Codespace y abre un terminal.
2. Comprueba que Node.js está instalado y cumple el requisito de versión:

   ```bash
   node --version
   ```

   Deberías ver la versión 24 o posterior.

3. Instala Copilot CLI globalmente:

   ```bash
   npm install -g @github/copilot
   ```

4. Comprueba la instalación:

   ```bash
   copilot --version
   ```

   Deberías ver el número de versión.

## Autentícate con GitHub

La primera vez que inicies Copilot CLI, te pedirá que te autentiques con tu cuenta de GitHub.

1. Inicia Copilot CLI:

   ```bash
   copilot
   ```

2. Si se te solicita, sigue las instrucciones de autenticación con código de dispositivo y autoriza Copilot CLI.
3. Copilot CLI muestra este mensaje:

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. Confirma que la ruta corresponde a tu repositorio de Tailspin Toys y responde que sí seleccionando **Yes, and remember this folder for future sessions**.

> [!NOTE]
> Puede que ya estés autenticado mediante tu sesión de GitHub en el Codespace. Si Copilot CLI se inicia sin pedirte que te autentiques, ya está todo listo.

## Familiarízate con el entorno

Los comandos que escribes en el prompt normal del shell se ejecutan directamente en tu Codespace. Cuando se inicia Copilot CLI, el lenguaje natural se envía al agente y los comandos de barra controlan la conversación.

1. Escribe `/model`, usa las teclas de dirección para seleccionar **Auto**, pulsa <kbd>Enter</kbd> y vuelve a pulsar <kbd>Enter</kbd> para confirmar.
2. Escribe `/help` para ver los comandos disponibles en tu versión y pulsa <kbd>Esc</kbd> para cerrar la ayuda.
3. Haz una pregunta sencilla a Copilot para comprobar que todo funciona:

   ```plaintext
   What are the key files in this project?
   ```

4. Lee la respuesta y observa cómo Copilot explora el repositorio antes de responder.
5. Escribe `/mcp list` y confirma que está disponible el servidor GitHub MCP integrado.
6. Pide a Copilot que encuentre la incidencia de filtrado:

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. Abre la URL y lee la incidencia. La usarás después de completar un primer cambio rápido.

> [!TIP]
> Una sesión normal de Copilot CLI trabaja en la rama que esté seleccionada en el terminal. No crea automáticamente un árbol de trabajo. Crearás una rama de funcionalidad antes de cada cambio.

## Usa el acceso directo del taller

Normalmente Copilot CLI pregunta antes de usar herramientas que quedan fuera de los permisos establecidos. En este taller volverás a iniciarlo con `--yolo`, un acceso directo aprobado por el usuario que elimina esos avisos dentro del Codespace para que puedas centrarte en los ejercicios.

> [!CAUTION]
> `--yolo` habilita los permisos automáticos completos (`--allow-all-tools`, `--allow-all-paths` y `--allow-all-urls`). Úsalo solo en un entorno aislado, como un Codespace o una máquina virtual, y no lo configures nunca como alias predeterminado para el desarrollo diario. Consulta [Permitir y denegar el uso de herramientas][allow-all-warning] para más información.

En este taller, `--enable-all-github-mcp-tools` habilita las herramientas de escritura y lectura de GitHub MCP que utilizarán las siguientes lecciones para trabajar con incidencias y solicitudes de incorporación de cambios. El Codespace limita el acceso a tu equipo local, pero los recursos de GitHub autenticados son reales. Revisa los cambios antes de publicarlos o combinarlos.

1. Sal de Copilot CLI con `/exit`.
2. Vuelve a iniciarlo desde la raíz del repositorio:

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. Haz otra pregunta breve sobre el proyecto para confirmar que la conversación funciona y, después, sal con `/exit`.

Copilot guarda las conversaciones automáticamente. Más adelante, cuando cambies las instrucciones o añadas un agente, usarás `copilot --resume` para volver a la misma conversación y rama de funcionalidad.

## Resumen y próximos pasos

¡Enhorabuena! En esta lección:

- Instalaste GitHub Copilot CLI con npm.
- Te autenticaste con tu cuenta de GitHub.
- Confiaste en el repositorio del taller y probaste una conversación breve.
- Encontraste la incidencia de filtrado mediante el servidor GitHub MCP integrado.

A continuación, [empezarás tu primer cambio específico][next-lesson] y usarás Copilot CLI para mostrar las valoraciones por estrellas en las tarjetas de juegos.

## Recursos

- [Instalar GitHub Copilot CLI][install-cli]
- [Acerca de GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
