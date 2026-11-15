---
title: "Ejercicio 1 - Instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Agente de Copilot en la nube →][next-lesson]

El contexto es fundamental cuando trabajas con IA generativa. Si una tarea debe realizarse de una forma concreta, o Copilot necesita conocer información de contexto, asegúrate de que pueda acceder a ella. Los [archivos de instrucciones][instruction-files] proporcionan esa orientación para que Copilot entienda no solo *qué* quieres que haga, sino también *cómo* quieres que lo haga.

En este ejercicio:

- Explorarás cómo llega a Copilot el contexto específico del proyecto, las directrices de programación y los estándares de documentación mediante las instrucciones personalizadas del repositorio y los archivos de instrucciones específicos para rutas.
- Añadirás un nuevo estándar para todo el repositorio en `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los entornos de VS Code y CLI, aquí no ejecutarás un prompt «antes y después». El agente de Copilot en la nube trabaja de forma asíncrona con incidencias de GitHub, por lo que es más difícil demostrar sus efectos comparándolos en tiempo real. Verás cómo influye el archivo de instrucciones cuando revises las solicitudes de incorporación de cambios que genere el agente en la nube en este recorrido.

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
2. Explora el archivo y fíjate en la breve descripción del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. Estas instrucciones se aplican a cualquier interacción que tengas con Copilot.
3. Abre la carpeta `.github/instructions` y explórala. Observa que hay instrucciones para archivos Astro, la capa de datos de Drizzle, pruebas y otros ámbitos.
4. Abre `.github/instructions/unit-tests.instructions.md`. Observa el campo `applyTo` del principio: define un patrón global (relativo a la raíz del repositorio) que determina a qué archivos se aplican las instrucciones. En este caso, coincidirá con cualquier archivo de pruebas TypeScript, por ejemplo, los que sigan el patrón `**/*.test.ts`.
5. Observa las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones (como `unit-tests.instructions.md`) y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones grandes en archivos más pequeños y reutilizables, y señalar a Copilot ejemplos que debe seguir al generar código. (Las rutas indicadas son relativas al archivo de instrucciones, no a la raíz del repositorio).

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación del proyecto, pero aún no exige que el código incluya documentación. En los pasos siguientes añadirás directrices sobre los comentarios TSDoc y los comentarios de cabecera de los archivos.

## Añade un estándar para todo el repositorio

El siguiente paso es la única edición que harás aquí: añade una regla para todo el proyecto que indique que el código debe incluir documentación en forma de comentarios TSDoc y comentarios de cabecera de archivo. El agente en la nube aplicará esta regla más adelante al trabajar en las incidencias que le asignes.

Antes de editar, prepara una rama de trabajo. El agente en la nube leerá las instrucciones de la rama a la que se dirija la incidencia, pero así también confirmarás los cambios de forma ordenada:

1. Desde el terminal de tu codespace, crea una rama nueva y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

Como hemos indicado, `.github/copilot-instructions.md` proporciona información del proyecto a Copilot. Documentemos los estándares de programación para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Observa cómo documenta los estándares de programación del proyecto. Aún no incluye ninguna directriz para la documentación en el código, por eso el asistente generado no tenía comentarios de documentación.
3. Añade las siguientes líneas de Markdown después de los estándares existentes. Así indicarás a Copilot que añada comentarios de cabecera y comentarios TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, puedes crear archivos de instrucciones en el nivel del repositorio (`.github/copilot-instructions.md`) para definir directrices generales, o archivos `*.instructions.md` para determinados lenguajes, tipos de archivos y tareas. El archivo del repositorio es el lugar adecuado para incluir estándares de todo el proyecto, como la nueva regla sobre los comentarios de documentación.

## Confirma, sube y combina la actualización de instrucciones

El agente en la nube lee los archivos de instrucciones de la rama a la que se dirige la incidencia. En el siguiente ejercicio asignarás una incidencia a Copilot, que creará una rama a partir de `main`. Por tanto, debes integrar las nuevas instrucciones en `main` para que el agente pueda usarlas.

1. Prepara y confirma los cambios:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Sube la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una solicitud de incorporación de cambios desde `custom-instructions-cloud` a `main` y combínala. Así, el agente en la nube podrá leer las instrucciones cuando trabaje en las incidencias que le asignes en el siguiente ejercicio.

> [!TIP]
> Si prefieres trabajar directamente en `main` durante este taller, puedes omitir los pasos de la rama y confirmar los cambios directamente en `main`. El taller utiliza una rama para reproducir el flujo habitual de un proyecto real.

## Resumen y próximos pasos

Has explorado cómo obtiene contexto Copilot de los archivos de instrucciones de este proyecto y has añadido un estándar nuevo para todo el repositorio en `.github/copilot-instructions.md`. Verás cómo se aplica ese estándar en las solicitudes de incorporación de cambios que genere el agente en la nube durante el resto de este recorrido.

A continuación, [asignarás tu primera incidencia al agente de Copilot en la nube][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
