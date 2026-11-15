---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo de agente →][next-lesson]

El contexto es fundamental cuando trabajas con IA generativa. Si una tarea debe realizarse de una forma concreta, o Copilot necesita conocer información de contexto, asegúrate de que pueda acceder a ella. Hay varias formas de compartir contexto específico con Copilot. Una de las principales son los [archivos de instrucciones][instruction-files], que proporcionan directrices para generar código.

En este ejercicio:

- Explorarás cómo llega a Copilot el contexto específico del proyecto, las directrices de programación y los estándares de documentación mediante las instrucciones personalizadas del repositorio y los archivos de instrucciones específicos para rutas.
- Generarás la primera parte de los datos para el filtrado (una función auxiliar para los editores) con las instrucciones *actuales*.
- Añadirás un nuevo estándar para todo el repositorio en `.github/copilot-instructions.md`.
- Volverás a ejecutar el mismo prompt y observarás cómo el código generado sigue el nuevo estándar.
- Confirmarás la actualización de instrucciones y la parte del filtrado en `main` para que Copilot pueda utilizar las nuevas directrices en el ejercicio siguiente.

> [!CAUTION]
> El código generado puede no seguir algunas de las normas que establezcas. Copilot no es determinista. El objetivo de este ejercicio es observar la *tendencia* de los cambios en el comportamiento tras actualizar las instrucciones, no obtener exactamente el mismo código carácter por carácter.

## Archivos de instrucciones

### Escenario

Como cualquier empresa de desarrollo competente, Tailspin Toys tiene directrices y requisitos para las prácticas de desarrollo. Entre ellos:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz de usuario debe usar un tema oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Al principio de cada archivo debe haber un bloque de comentarios que describa su función.

Con los archivos de instrucciones te asegurarás de que Copilot disponga de la información adecuada para realizar las tareas conforme a estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que entienda mejor tu estilo y tus requisitos de programación. Te permiten orientar a Copilot para que genere sugerencias y fragmentos de código más útiles. Puedes especificar tus convenciones de programación, bibliotecas e incluso el tipo de comentarios que quieres. Puedes crear instrucciones para todo el repositorio o para tipos concretos de archivos y tareas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`: un único archivo que se envía a Copilot en **todas** las solicitudes para el repositorio. Debe contener información de contexto relevante para la mayoría de las solicitudes que se envían a Copilot en el chat o en la CLI. Puede describir la pila tecnológica, el proyecto, las prácticas recomendadas y otras directrices generales.
- `.github/instructions/*.instructions.md`: archivos que puedes crear para tareas o tipos de archivos específicos. Por ejemplo, sirven para proporcionar directrices para determinados lenguajes, como TypeScript o Astro, o para tareas como crear un componente de interfaz de usuario o un conjunto de pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se usan para generar código en Copilot Chat, no para completar código ni sugerir ediciones siguientes.
>
> Copilot Chat, Copilot CLI y el agente de Copilot en la nube usan los archivos de instrucciones del repositorio y los archivos `*.instructions.md` (con frontmatter `applyTo`) al generar código.
>
> Copilot también [admite archivos de instrucciones en otros formatos][custom-instructions-support], como AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para gestionar archivos de instrucciones

En este taller no hay espacio para explicar en profundidad cómo crear archivos de instrucciones. Sin embargo, los ejemplos del proyecto muestran un enfoque representativo. A grandes rasgos:

- Enfoca `copilot-instructions.md` en directrices para el proyecto, como una descripción de lo que se está desarrollando, la estructura del proyecto y los estándares generales de programación.
- Usa archivos `*.instructions.md` para proporcionar instrucciones específicas sobre tipos de archivos (pruebas unitarias, componentes de Astro y la capa de datos) o tareas concretas.
- Escribe las instrucciones en lenguaje natural. Procura que sean claras e incluye ejemplos del aspecto que debe y no debe tener el código.

No hay una única manera correcta de crear archivos de instrucciones, igual que tampoco hay una sola manera de usar la IA. La experimentación te ayudará a encontrar el enfoque que mejor se adapte a tus entornos y escenarios.

> [!TIP]
> Todos los proyectos que usan GitHub Copilot deberían disponer de un conjunto sólido de archivos de instrucciones. Al explorar los archivos de este proyecto, puede que observes que incluyen diversos tipos de tareas, como [actualizaciones de la interfaz de usuario][ui-instructions] y archivos [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece esta opción de una forma distinta (por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI). La lección del entorno que estés usando la explicará cuando sea pertinente.
>
> ¿Buscas plantillas o un punto de partida? Explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizadas del proyecto

Dedica un momento a leer los archivos de instrucciones incluidos en este repositorio: hay un archivo principal `copilot-instructions.md` y una colección de archivos `*.instructions.md` para distintas tareas. Ábrelos en tu editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la breve descripción del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. Dentro de **Code standards**, observa las directrices anidadas para **GitHub Actions Workflows**. Se aplican a cualquier interacción que tengas con Copilot.
3. Abre la carpeta `.github/instructions` y explórala. Observa que hay instrucciones para archivos Astro, la capa de datos de Drizzle, pruebas y otros ámbitos.
4. Abre `.github/instructions/unit-tests.instructions.md`. Observa el campo `applyTo` del principio: define un patrón global (relativo a la raíz del repositorio) que determina a qué archivos se aplican las instrucciones. En este caso, coincidirá con cualquier archivo de pruebas TypeScript, por ejemplo, los que sigan el patrón `**/*.test.ts`.
5. Observa las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones (como `unit-tests.instructions.md`) y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones grandes en archivos más pequeños y reutilizables, y señalar a Copilot ejemplos que debe seguir al generar código. (Las rutas indicadas son relativas al archivo de instrucciones, no a la raíz del repositorio).

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación del proyecto, pero aún no exige que el código incluya documentación. En los pasos siguientes añadirás directrices sobre los comentarios TSDoc y los comentarios de cabecera de los archivos.

## Crea una rama para los cambios

Sigamos algunas prácticas recomendadas y creemos una rama para los cambios.

1. Vuelve al codespace del ejercicio anterior.
2. Abre un terminal nuevo pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama nueva y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Usa Copilot Chat *antes* de actualizar las instrucciones

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve a tu codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. La selección de modo y modelo varía según el ejercicio; cada paso lo indica cuando es pertinente.

Para observar el efecto de las instrucciones personalizadas, empieza enviando un prompt con el archivo de instrucciones actual. Más adelante, lo actualizarás y volverás a enviar el mismo prompt para comparar los resultados.

1. Cierra las pestañas abiertas de ejercicios anteriores para que Copilot solo use el contexto necesario.
2. Abre `src/lib/publishers.ts` para que Copilot sepa dónde debe ir la función auxiliar.
3. Selecciona **Agent** en el menú desplegable de agentes de la vista Chat para que Copilot pueda aplicar cambios a los archivos.

   ![Selector de agente de la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Envía el siguiente prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explorará el proyecto y aplicará cambios al código, que normalmente incluirán el archivo de la función auxiliar y sus pruebas.
6. Observa que la función auxiliar propuesta está tipada, recibe un cliente `db` como primer argumento y devuelve una matriz de editores tipada. Estas características proceden de las convenciones de la capa de datos de `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Observa que el código propuesto **no incluye** comentarios TSDoc ni comentarios de cabecera.

> [!CAUTION]
> Copilot no es determinista, por lo que podría añadir comentarios de documentación aunque no se lo pidas. No pasa nada: el objetivo es observar la *uniformidad* de los resultados después de actualizar las instrucciones.

## Añade un estándar para todo el repositorio

Como hemos indicado, `.github/copilot-instructions.md` proporciona información del proyecto a Copilot. Documentemos los estándares de programación para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Observa cómo documenta los estándares de programación del proyecto. Aún no incluye ninguna directriz para la documentación en el código, por eso la función auxiliar generada no tenía comentarios de documentación.
3. Añade las siguientes líneas de Markdown después de los estándares existentes. Así indicarás a Copilot que añada comentarios de cabecera y comentarios TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, puedes crear archivos de instrucciones en el nivel del repositorio (`.github/copilot-instructions.md`) para definir directrices generales, o archivos `*.instructions.md` para determinados lenguajes, tipos de archivos o tareas. El archivo del repositorio es el lugar adecuado para incluir estándares de todo el proyecto, como la nueva regla sobre los comentarios de documentación.

## Vuelve a ejecutar el prompt y observa los cambios

1. Vuelve a Copilot Chat y selecciona **New Chat** para borrar el contexto anterior.
2. Vuelve a seleccionar `src/lib/publishers.ts` para que Copilot se centre en el archivo correcto.
3. Envía el **mismo prompt** que antes:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observa que el archivo propuesto ahora empieza con un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observa que la función propuesta ahora incluye un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Acabas de orientar a Copilot para que siga un estándar nuevo y lo aplique al código real sobre el que trabajarás en el ejercicio siguiente.

## Confirma las instrucciones y sube la rama

Los archivos de instrucciones son recursos del repositorio, igual que los demás archivos, así que se gestionan con el mismo sistema de control de versiones. Confirmemos los cambios y subamos la rama.

1. Abre una ventana de terminal nueva pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Desde el terminal, ejecuta el siguiente comando para comprobar que están los cambios de instrucciones y de la función auxiliar:

   ```bash
   git status
   ```

3. Desde el terminal, prepara y confirma la actualización de instrucciones y todos los archivos modificados por Copilot para crear la función auxiliar:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra otros cambios relacionados, como pruebas, añádelos también.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Sube la rama al repositorio:

   ```bash
   git push -u origin custom-instructions
   ```

## Crea y combina una solicitud de incorporación de cambios

Ya has subido la rama, así que debemos crear una solicitud de incorporación de cambios y asociarla a la incidencia del backlog sobre la documentación. Podríamos hacerlo manualmente, pero Copilot puede encargarse de la tarea con las herramientas de GitHub ya conectadas al proyecto. Pidámosle que encuentre la incidencia, cree una PR para cerrarla y, después, la combine.

> [!NOTE]
> En este ejercicio, Copilot actuará por primera vez en GitHub en tu nombre. La plantilla del proyecto ya conecta las herramientas de GitHub a tu espacio de trabajo, así que no hay que configurar nada. Cuando Copilot use una de ellas por primera vez, es posible que VS Code te pida iniciar sesión en GitHub. Sigue las instrucciones para autorizarlo. En un ejercicio posterior aprenderás cómo funciona esta conexión (Model Context Protocol).

1. Abre Copilot Chat en tu codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y comprueba que **Agent** está seleccionado en el selector de agentes.
3. Pide a Copilot que encuentre la incidencia relacionada con la actualización de los archivos de instrucciones y que cree una PR desde la rama actual. Indícale que describa tanto las actualizaciones de instrucciones como la nueva función auxiliar de editores:

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot empezará a buscar la incidencia y a crear la PR.
5. Si Copilot te pide **Allow** para realizar acciones de GitHub en tu nombre, revisa el comando y selecciona **Allow** si procede.
6. Cuando se cree la PR, pide a Copilot que la combine y vuelva a cambiar la rama local a `main` con el siguiente prompt:

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Si Copilot te pide **Allow** para realizar acciones de GitHub y ejecutar comandos de shell en tu nombre, revisa el comando y selecciona **Allow** si procede.

¡Has creado y combinado una solicitud de incorporación de cambios con ayuda de GitHub Copilot!

## Resumen y próximos pasos

Has explorado cómo obtiene contexto Copilot de los archivos de instrucciones de este proyecto y has usado Copilot Chat en VS Code para:

- Enviar un prompt para generar código y observar el resultado con las instrucciones *actuales*.
- Añadir un nuevo estándar para todo el repositorio en `.github/copilot-instructions.md`.
- Volver a ejecutar el mismo prompt y observar cómo el código propuesto sigue el nuevo estándar.
- Confirmar la actualización de instrucciones y la base de la función auxiliar en `main` para que el siguiente ejercicio pueda aprovechar esos cambios.

A continuación, aplicarás las instrucciones en el [modo de agente][next-lesson] mientras Copilot añade una funcionalidad nueva en varios archivos del proyecto.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personalizadas para tu cuenta de GitHub Copilot][personal-instructions]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
