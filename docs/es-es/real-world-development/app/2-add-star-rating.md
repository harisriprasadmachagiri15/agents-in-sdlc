---
title: "Lección 2 - Añadir valoraciones con estrellas: una mejora rápida"
description: "Inicia tu primera sesión de agente en la aplicación GitHub Copilot, realiza un pequeño cambio en las tarjetas de los juegos y combínalo como tu primera solicitud de incorporación de cambios."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Ya has explorado el espacio de trabajo y has probado un chat rápido. Ahora iniciarás una **sesión de agente** y harás tu primer cambio en el proyecto. Será pequeño: los juegos ya tienen una valoración con estrellas en sus datos, pero las tarjetas de la página de inicio aún no la muestran. Pedirás al agente que la muestre, revisarás el cambio y lo combinarás como tu primera solicitud de incorporación de cambios.

En esta lección:

- iniciarás una sesión de agente y aprenderás cómo se estructura.
- pedirás al agente que realice un cambio pequeño y acotado en el proyecto.
- revisarás el cambio en la vista de diferencias del espacio de trabajo.
- ejecutarás la aplicación en local para comprobar el cambio en el navegador.
- abrirás y combinarás tu primera solicitud de incorporación de cambios.

## Escenario

Cada juego de Tailspin Toys puede tener una valoración con estrellas, que ya aparece en la página de detalles. Sin embargo, las tarjetas de la página de inicio solo muestran el título, la categoría, el editor y la descripción. Como ejercicio inicial, pedirás al agente que muestre la valoración existente en cada tarjeta: un cambio pequeño y autocontenido, ideal para tu primera sesión.

## Anatomía de una sesión

Una **sesión** es una conversación con un agente que se ejecuta en su propio espacio de trabajo aislado. Cada sesión recibe un **árbol de trabajo y una rama de Git dedicados**, lo que permite ejecutar varias sesiones a la vez —por ejemplo, una para añadir una funcionalidad y otra para corregir un error— sin que sus cambios entren en conflicto. Las sesiones aparecen en la barra lateral, agrupadas por repositorio; selecciona una para cambiar a ella.

Dentro de una sesión verás tres elementos: la **conversación** con el agente, la **actividad de las herramientas** mientras explora y edita archivos, y la lista de **archivos modificados** con sus diferencias.

## Iniciar una sesión y solicitar el cambio

Inicia una sesión nueva para explorar el proyecto e implementar la funcionalidad. Durante la [configuración de la aplicación][prior-lesson], añadiste el proyecto desde su repositorio de GitHub. Ahora crearás una sesión para ese repositorio y solicitarás el cambio.

1. Vuelve a la aplicación GitHub Copilot o ábrela.
2. Selecciona **+** en la parte superior de la barra lateral para iniciar una sesión.
3. Selecciona el repositorio `tailspin-toys`.
4. Elige un **new working tree** y el modo **Interactive** debajo del cuadro de mensaje. Solicita el cambio con este mensaje:

    ```plaintext
    Muestra la propiedad starRating sobre 5 en las tarjetas de juegos de la página de listado. Si la valoración es null, muestra "No rating yet". Mantén el diseño de las tarjetas, añade pruebas y ejecuta las comprobaciones pertinentes.
    ```

5. Pulsa <kbd>Enter</kbd> para enviar el mensaje a Copilot.

La aplicación Copilot empieza creando un árbol de trabajo nuevo, una copia aislada del proyecto. Después, explora el proyecto, localiza los archivos que debe actualizar para añadir la funcionalidad y crea el código necesario. Ya has añadido una funcionalidad con la aplicación Copilot.

## Revisar las diferencias

Revisa todos los cambios generados por inteligencia artificial (IA) antes de combinarlos, aunque sean pequeños. Examínalos en la aplicación Copilot.

1. En la esquina superior derecha de la aplicación, selecciona **Toggle review panel**. En el panel que se abre, selecciona **Changes** para ver los archivos modificados y sus diferencias.

    ![Barra superior de la aplicación GitHub Copilot con una flecha que señala el botón Toggle review panel, a la derecha de Create PR](../../../_images/app-2-review-panel.png)

2. Deberías ver código añadido a `GameCard.astro`, el archivo principal que muestra los detalles de los juegos. Debería ser parecido al siguiente: un bloque pequeño que muestra la valoración cuando existe y utiliza `No rating yet` cuando `starRating` es `null`.

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> Copilot, como cualquier herramienta de IA generativa, ofrece resultados probabilísticos, no deterministas. El código exacto puede variar respecto al ejemplo, pero debería ser parecido.

## Comprobar los cambios

Antes de abrir un navegador, revisa los resultados de las comprobaciones automatizadas del agente. Confirma que las pruebas cubren una valoración numérica `starRating` y el caso alternativo `null`. Si falta un requisito previo o se ha omitido una comprobación, no la des por superada; revisa cualquier solicitud de instalación antes de aprobarla.

No basta con leer el código y dar por hecho que funciona. Pide a Copilot que abra el sitio web para poder examinar la interfaz actualizada. Para ello, inicia el sitio y ábrelo en un canvas de navegador.

> [!TIP]
> Un canvas es un widget interactivo disponible dentro de la aplicación Copilot. Más adelante explorarás canvases personalizados e incluso crearás uno, pero por ahora utilizarás el canvas de navegador integrado.

1. Pide a Copilot que inicie la aplicación y abra la página en el canvas de navegador:

    ```plaintext
    Inicia la aplicación y ábrela en el canvas del navegador.
    ```

2. En unos instantes, se iniciará la aplicación y se abrirá una ventana del navegador dentro de la aplicación Copilot.
3. Confirma que las tarjetas de los juegos con valoración muestran su puntuación sobre cinco.
4. Cuando termines, pide a Copilot que detenga el servidor de desarrollo que ha iniciado para esta sesión:

    ```plaintext
    Detén el servidor de desarrollo y cierra el canvas del navegador.
    ```

## Abrir y combinar tu primera solicitud de incorporación de cambios

Ya has creado la funcionalidad. Ahora abrirás una solicitud de incorporación de cambios (PR) para combinar el código nuevo con el código existente.

1. Selecciona **Create PR** en la esquina superior derecha.
2. Si se te solicita, selecciona **Sign in with your browser** y sigue las indicaciones para autenticarte.
3. Si Copilot indica que la política del repositorio exige asociar una incidencia antes de crear la PR, confirma que debe seguir ese proceso.
4. Copilot empezará a crear la PR.
5. Selecciona la burbuja **PR** situada justo encima del chat para abrir la solicitud en el panel de revisión. Puedes revisar la PR desde allí.
6. Cuando estés conforme, selecciona **Ready to merge**.
7. En el nuevo diálogo, selecciona **Merge pull request** para combinar la solicitud.

## Resumen y próximos pasos

¡Enhorabuena! Has publicado tu primer cambio con la aplicación GitHub Copilot. En concreto:

- iniciaste una sesión de agente y aprendiste cómo se estructuran las sesiones.
- pediste al agente que hiciera un cambio pequeño y acotado en las tarjetas de juegos.
- revisaste el cambio en la vista de diferencias del espacio de trabajo.
- ejecutaste la aplicación en local para comprobar las valoraciones con estrellas en el navegador.
- abriste la primera PR, revisaste sus comprobaciones y autorizaste explícitamente su combinación.

A continuación, [partirás de la incidencia de filtrado y utilizarás los modos Plan y Autopilot][next-lesson] para crear una funcionalidad más amplia.

## Recursos

- [Trabajar con sesiones de agente en la aplicación GitHub Copilot][agent-sessions]
- [Acerca de la aplicación GitHub Copilot][about-copilot-app]
- [Administrar incidencias y solicitudes de incorporación de cambios con la aplicación GitHub Copilot][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#instalar-y-configurar-la-aplicación-github-copilot
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
