---
title: "Ejercicio 4 - Supervisar y administrar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has pedido al agente de Copilot en la nube que realice tres tareas distintas para mejorar la experiencia y añadir funcionalidades. Aunque el agente trabaja de forma autónoma y asíncrona, sigue siendo importante poder supervisar esas tareas.

Hay varias herramientas para administrar las tareas asignadas al agente, como [la página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver las tareas de los agentes que tienen solicitudes de incorporación de cambios (PR) abiertas, explorar las operaciones realizadas y orientar las sesiones que aún están en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- dirigirás una sesión en curso para pedir una funcionalidad adicional.

## Escenario

Después de asignar al agente la tarea de crear un modo de alto contraste, el equipo decidió que también sería buen momento para añadir un modo claro. Como ya se estaba actualizando el estilo del sitio y añadiendo un conmutador, parecía razonable incluir esta función. Dirigirás el trabajo del agente para que añada el modo claro además del modo de alto contraste.

## Revisar las tareas del agente de Copilot en la nube

Revisa el estado actual de todas las tareas asignadas al agente de Copilot en la nube.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa las listas de tareas del panel principal y del panel izquierdo. Deberías ver las tareas asignadas a Copilot, entre ellas:
   - actualizar la documentación del código.
   - generar interfaces de programación de aplicaciones (API) para modificar juegos.
   - añadir un modo de alto contraste al sitio.
3. Selecciona una de las tareas en curso. Revisa las operaciones que Copilot ha realizado. Por ejemplo:
   - obtener el código del repositorio.
   - crear el entorno de trabajo de Copilot.
   - configurar servidores MCP.
   - realizar distintas tareas para completar el trabajo asignado.

> [!NOTE]
> Los pasos exactos variarán según el estado del trabajo de Copilot y el enfoque que haya seguido.

4. Observa también el panel de solicitudes de incorporación de cambios (PR) de la derecha. Permite consultar la PR y los archivos modificados para seguir el trabajo.

## Dirigir el agente en la nube

Ya has visto el estado de las tareas activas. Ahora pedirás a Copilot que añada el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre que le haya dado Copilot y del estado actual del trabajo.

    ![Sesión de accesibilidad en el centro de control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado de configurarse y ha empezado a trabajar. Lo sabrás cuando empiecen a aparecer mensajes parecidos a los siguientes.
3. En el diálogo **Steer active session while Copilot is working**, escribe este mensaje:

    ```plaintext
    Mientras implementamos el modo de alto contraste, añade también un modo claro. Debe haber un conmutador para este modo, de forma que cada usuario pueda elegir la apariencia que prefiera.
    ```

    ![Captura de la tarea del agente en la nube en la página de agentes, con el diálogo para dirigir la sesión activa de Copilot resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el mensaje.
5. Observa cómo Copilot reconoce la solicitud y la incorpora al flujo de trabajo.

## Dejar que Copilot complete la tarea

Como antes, Copilot seguirá trabajando en la tarea actualizada. Incorporará la nueva solicitud al flujo cuando termine el paso que estaba realizando al recibir el mensaje.

El proceso puede tardar varios minutos; puedes aprovechar para repasar todo lo que has aprendido y explorado hasta ahora.

## Resumen y próximos pasos

En esta lección has explorado la página de agentes de Copilot, el centro principal para supervisar y orientar las tareas del agente de GitHub Copilot en la nube. Puedes usar este centro de control para seguir las tareas activas y completadas, revisar el trabajo y redirigir las sesiones en curso para ajustar el alcance o proporcionar instrucciones.

Has explorado estos conceptos:

- explorar el centro de control y la página de agentes para supervisar las tareas del agente en la nube.
- redirigir una sesión en curso para pedir funcionalidades adicionales.

Mientras Copilot termina de trabajar en las funciones de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
