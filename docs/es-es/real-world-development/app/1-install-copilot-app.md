---
title: "Lección 1 - Instalar la aplicación GitHub Copilot"
description: "Instala la aplicación GitHub Copilot, conecta el repositorio que has creado a partir de la plantilla, familiarízate con el espacio de trabajo y prueba un chat rápido."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La **[aplicación GitHub Copilot][about-copilot-app]** es una aplicación de escritorio para el desarrollo dirigido por agentes. Se basa en GitHub Copilot CLI y se integra de forma nativa con GitHub, por lo que los repositorios, las ramas y las canalizaciones de CI funcionan sin configuración adicional. Está diseñada para flujos de trabajo en los que diriges varios agentes en paralelo, cada uno en su propio espacio de trabajo aislado, en lugar de hacer todo el trabajo por tu cuenta, y automatizas tareas repetitivas. Con Node.js instalado y tu copia del proyecto preparada, el siguiente paso es instalar la aplicación y conectar ese repositorio.

En esta lección:

- Instala la aplicación GitHub Copilot e inicia sesión.
- Añade el proyecto a la aplicación desde su repositorio de GitHub.
- Familiarízate con el espacio de trabajo, incluido el backlog que ha creado la plantilla.
- Prueba un chat rápido para conocer la propia aplicación.

## Escenario

Tu equipo está adoptando agentes de IA para abordar un backlog cada vez mayor. La aplicación Copilot ofrece un único lugar desde el que dirigir ese trabajo: seleccionar incidencias, ejecutar agentes, revisar cambios y combinar solicitudes de incorporación de cambios. En esta lección instalarás y conectarás la aplicación, y aprenderás a iniciar una conversación sobre el proyecto.

> [!NOTE]
> Necesitas un plan de Copilot válido: Copilot Free, Copilot Student o cualquier plan de pago (Pro, Pro+, Business o Enterprise). Si utilizas Copilot Business o Copilot Enterprise, el administrador debe mantener habilitada la directiva **GitHub Copilot app**.

## Instala y configura la aplicación GitHub Copilot

El primer paso para utilizar la aplicación GitHub Copilot es instalarla. Hay versiones para Windows, macOS y Linux. Vamos a instalar la aplicación, autenticarnos y añadir el repositorio de Tailspin Toys.

1. En un navegador, abre la [página de inicio de la aplicación GitHub Copilot][download-app].
2. Descarga la aplicación para tu plataforma e instálala siguiendo las instrucciones de la página.
3. Abre la aplicación después de instalarla.
4. Selecciona **Sign in to GitHub** y sigue las indicaciones para iniciar sesión con una cuenta de GitHub.com o GHE.com. Si utilizas GitHub Enterprise Cloud con residencia de datos, elige **GitHub Enterprise Cloud** e introduce el nombre de host `*.ghe.com` cuando se solicite.

   Si utilizas GitHub Enterprise Server, completa primero la incorporación con tu cuenta en la nube. Después, abre **Settings**, selecciona **Accounts**, selecciona **Add account** y elige **GitHub Enterprise Server**. Sigue las indicaciones para autorizar la aplicación en el navegador, como se describe en la [guía de inicio de sesión en Enterprise Server][ghes-sign-in].

5. Si durante la configuración se te pide que selecciones un repositorio, elige la copia de Tailspin Toys que has creado a partir de la plantilla. También puedes omitir este paso y añadir el proyecto cuando termine la configuración.
6. Cuando se te solicite, elige un tema y termina el proceso de configuración.
7. Busca el repositorio en **Sessions**, en la barra lateral. Si ya aparece, no tienes que volver a añadirlo.
8. Si todavía no está conectado, selecciona **+** junto a **Sessions** para crear una sesión y, después, selecciona **Add GitHub repository**. Indica el propietario y el nombre del repositorio que anotaste en la lección de requisitos previos.
9. Comprueba que el repositorio aparece en la barra lateral. En la disposición que se muestra aquí, las carpetas de repositorio, como `tailspin-toys`, están agrupadas en **Sessions**, y cada repositorio contiene sus sesiones.

## Familiarízate con el espacio de trabajo

Con el proyecto conectado, dedica un momento a conocer el espacio de trabajo. La aplicación organiza todo en varias áreas de la barra lateral:

- **New**: como cabe esperar, aquí puedes iniciar una sesión nueva de chat con Copilot.
- **Pull requests**: tus solicitudes de incorporación de cambios, disponibles mediante la integración nativa con GitHub de la aplicación. Desde aquí puedes examinar y filtrar las solicitudes, comprobar el estado de CI y revisarlas sin salir de la aplicación.
- **Issues**: tus incidencias, disponibles del mismo modo. Desde aquí puedes examinarlas y filtrarlas e iniciar una sesión a partir de una incidencia.
- **Automations**: tareas de agente guardadas que se ejecutan según una programación o bajo demanda. Son útiles para gestionar listas de tareas, realizar el mantenimiento periódico del proyecto o delegar otras tareas tediosas. El repaso final las ofrece como paso siguiente, no como otro ejercicio del taller.
- **Customize**: añade funciones a la aplicación Copilot mediante servidores MCP, plugins, habilidades y otros componentes. La usarás para configurar Playwright MCP.
- **Chats**: conversaciones ligeras para preguntas y lluvias de ideas que no necesitan una rama ni un espacio de trabajo propios. Esta sección aparece después de iniciar el primer chat. Si no está visible, selecciona **+** junto a **Sessions** para iniciar un chat.
- **Sessions**: agrupa las carpetas de repositorio y las sesiones de agente. Selecciona un nombre de repositorio, como `tailspin-toys`, para abrir la vista del repositorio, o selecciona una sesión debajo para volver a la conversación. Cada sesión se ejecuta en su propio espacio de trabajo aislado, así que puedes ejecutar varias sin que sus cambios entren en conflicto. Iniciarás tu primera sesión cuando añadas las valoraciones por estrellas.

A lo largo del taller, explorarás el espacio de trabajo.

> [!TIP]
> Si tienes dudas, pregunta a Copilot. Si no sabes cómo hacer algo o si es posible, puede ayudarte a orientarte.

### Localiza el backlog inicial

Prácticamente todos los proyectos tienen trabajo pendiente, y Tailspin Toys no es una excepción. Exploremos el backlog que se generó al crear el repositorio a partir de la plantilla.

1. En **Sessions**, en la barra lateral, selecciona la carpeta del repositorio Tailspin Toys, no una sesión que esté debajo. Después, selecciona **Issues** en la navegación superior del repositorio.
2. Busca estas incidencias por su título en lugar de dar por hecho su número:

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. Selecciona una incidencia para leer sus detalles. Cada incidencia también sirve como punto de partida para una sesión de agente. Partirás de la incidencia de filtrado después de completar un primer cambio rápido.

> [!NOTE]
> Las listas de **Issues** y **Pull requests** se filtran automáticamente para mostrar solo los elementos de los repositorios que has añadido a la aplicación Copilot. Para ver elementos de trabajo de otros repositorios, añádelos a la aplicación.

## Prueba un chat rápido

Una buena forma de familiarizarte con la aplicación es utilizarla para conocer la *propia aplicación*, y un **chat rápido** es justo lo que necesitas. Los chats rápidos permiten formular una pregunta o plantear ideas sin crear una rama ni un árbol de trabajo, por lo que son perfectos para una consulta rápida que no requiere una sesión.

1. En la barra lateral, selecciona **+** junto a **Chats** para abrir un chat nuevo. Si **Chats** todavía no está visible, selecciona **+** junto a **Sessions** para iniciar el primer chat; **Chats** aparecerá después.
2. Pregunta a la aplicación cómo funcionan sus sesiones:

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. Lee la respuesta en la vista de conversación. Verás que cada sesión se ejecuta en su propio árbol de trabajo de Git aislado, lo que permite ejecutar varios agentes en paralelo sin que sus cambios entren en conflicto. Puedes continuar la conversación o iniciar un chat nuevo en cualquier momento.

## Resumen y próximos pasos

¡Enhorabuena! Has instalado la aplicación GitHub Copilot, conectado el proyecto y explorado el espacio de trabajo. Has aprendido a:

- Instalar la aplicación e iniciar sesión en GitHub.
- Añadir un proyecto desde su repositorio de GitHub.
- Familiarizarte con el espacio de trabajo y localizar el backlog inicial en **Issues**.
- Usar un chat rápido para formular una pregunta breve y puntual.

A continuación, [iniciarás tu primera sesión de agente][next-lesson] y la usarás para mostrar las valoraciones por estrellas en las tarjetas de juegos.

## Recursos

- [Acerca de la aplicación GitHub Copilot][about-copilot-app]
- [Introducción a la aplicación GitHub Copilot][getting-started]
- [Trabajar con sesiones de agente en la aplicación GitHub Copilot][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
[ghes-sign-in]: https://docs.github.com/enterprise-server@3.22/copilot/copilot-on-ghes/sign-in-on-copilot-app
