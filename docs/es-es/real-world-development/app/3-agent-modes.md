---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Explora los modos de agente: utiliza Plan para acordar un enfoque, Autopilot para crear el filtrado a partir de una incidencia e Interactive para revisar y verificar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Empezamos por añadir una pequeña funcionalidad al proyecto. Sin embargo, los cambios más amplios requieren un proceso más sólido. La aplicación GitHub Copilot está diseñada para adaptarse al flujo existente de una organización y ayudarte a crear las cosas adecuadas de la forma correcta. En esta lección y las siguientes seguirás un proceso habitual de desarrollo con agentes: partirás de una incidencia para crear una funcionalidad, comprobarás el código, validarás el comportamiento y, finalmente, lo combinarás con el proyecto.

> [!NOTE]
> Utilizarás la misma sesión durante todo el flujo de la funcionalidad. Normalmente se usarían sesiones o solicitudes de incorporación de cambios (PR) distintas para los diferentes tipos de archivo, pero aquí simplificaremos el proceso para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una sesión de agente nueva a partir de una incidencia de GitHub.
- definirás los requisitos en el modo **Plan**.
- implementarás la funcionalidad con el modo **Autopilot**.
- revisarás el código.
- validarás manualmente la funcionalidad en un canvas de navegador.

Mientras continúas con esta funcionalidad, actualizarás las instrucciones del repositorio, personalizarás la habilidad existente `quality-checks`, añadirás validación con el Protocolo de contexto de modelo (MCP), crearás un agente de aseguramiento de la calidad (QA) y abrirás la PR de la funcionalidad.

## Escenario

El catálogo de Tailspin Toys crece y las personas necesitan filtrar los juegos por categoría y editor. La incidencia del backlog describe la funcionalidad, pero hay detalles —como la combinación de categorías— que deben acordarse antes de escribir código. Utilizarás el modo Plan para decidirlos y después autorizarás una implementación acotada con Autopilot.

## Contexto

Incorporar agentes de programación con inteligencia artificial (IA) al flujo de desarrollo no cambia los fundamentos. De hecho, los hace aún más importantes. La mayoría de los desarrolladores sigue un flujo parecido a este:

1. Abrir una incidencia que describa el trabajo pendiente.
2. Crear un plan de lo que hay que desarrollar.
3. Crear y revisar el código.
4. Ejecutar las pruebas para validar el código.
5. Validar manualmente la nueva funcionalidad.
6. Crear una solicitud de incorporación de cambios (PR).
7. Combinar el código cuando se haya revisado y el proceso de integración continua haya finalizado correctamente.

> [!NOTE]
> Los detalles varían según el equipo y la organización, pero la mayoría de los flujos siguen un patrón parecido.

Si mantienes este enfoque habitual, te asegurarás de que el código generado por IA cumple los requisitos y pasa por el mismo proceso de revisión que el código escrito manualmente.

## Modos de sesión

El **modo de sesión** controla el grado de autonomía del agente. Puedes seleccionarlo en el menú desplegable situado debajo del campo de mensaje y cambiarlo en cualquier momento:

- **Interactive**: trabajas con el agente. Este propone cambios y espera tu respuesta antes de continuar.
- **Plan**: el agente crea primero un plan. Revisas y apruebas el plan antes de que el agente lo ejecute.
- **Autopilot**: el agente trabaja de forma totalmente autónoma: escribe código, ejecuta pruebas e itera sin esperar indicaciones.

Empieza en el modo Plan, revisa el plan y, después, utiliza Autopilot para implementarlo.

## Iniciar una sesión desde la incidencia

Antes de empezar, confirma que la PR de las valoraciones con estrellas se ha combinado y que tu `main` local está actualizado.

1. En **Sessions**, en la barra lateral, selecciona la carpeta del repositorio de Tailspin Toys y, después, selecciona **Issues** en la navegación superior del repositorio. Abre **Allow users to filter games by category and publisher**. Esta incidencia ya existe en la plantilla; no hace falta crear otra.
2. Selecciona **New session** y elige un **new working tree** basado en `main` actualizado.

    ![Vista de una incidencia en la aplicación GitHub Copilot con una flecha que señala el botón New session](../../../_images/app-new-session-from-issue.png)

3. Confirma que la incidencia está adjunta a la sesión y selecciona **Plan** en el selector de modo.

> [!NOTE]
> Si no se encuentra Node.js o npm, pide al agente que ejecute `node --version` y `npm --version`. Si funcionan en el terminal, pero no en la sesión, comprueba la variable `PATH` o la configuración del administrador de versiones y, después, cierra completamente y vuelve a abrir la aplicación. Reintenta la operación y las comprobaciones fallidas; no reinstales npm solo porque el agente no lo encuentre.

## Planificar la funcionalidad de filtrado

La planificación te permite revisar el enfoque antes de que Copilot escriba código. Como has iniciado la sesión desde la incidencia, Copilot ya tiene la solicitud como contexto.

1. Envía este mensaje a Copilot:

   ```plaintext
   Implementa esta funcionalidad.
   ```

2. Responde a las preguntas de Copilot y compara el plan con los criterios de aceptación de la incidencia. Comprueba que incluye el filtrado por categoría y editor, controles accesibles, cambios de acceso a datos y pruebas. Aclara cualquier comportamiento poco definido, como la combinación de varias categorías o qué ocurre cuando ningún juego coincide.
3. Confirma que el plan incluye lint, pruebas unitarias, pruebas de extremo a extremo (E2E) y comprobación de tipos con las herramientas existentes del proyecto.
4. Asegúrate de que el plan se centre en implementar y probar el filtrado; crearás la PR después de completar el flujo de calidad.
5. Pide los cambios necesarios antes de aprobar el plan y conserva la URL de la incidencia y las aclaraciones acordadas para validarlas más adelante.

## Aprobar Autopilot explícitamente

Cuando estés conforme con el plan, selecciona **Approve and implement this plan** o la opción equivalente de tu versión. Comprueba el modo activo en lugar de dar por hecho que la aprobación cambia de modo. Confirma que el indicador muestra **Autopilot**.

Copilot empezará a implementar la funcionalidad. Observarás cómo sigue el plan, genera código y ejecuta pruebas.

> [!NOTE]
> La aprobación puede iniciar la implementación de inmediato, así que revisa primero el plan. Si Copilot indica que faltan dependencias o que hay un conflicto de puertos, resuelve el problema de configuración antes de dar las comprobaciones por terminadas. Detén solo los servidores que hayas iniciado.

## Revisar y verificar la implementación

Revisa el código generado antes de combinarlo, como harías con cualquier otro cambio. Examina el código y ejecuta el sitio para comprobar que todo funciona.

1. Abre **Changes** e inspecciona la implementación del filtrado y las pruebas.
2. Compara el resultado con la incidencia y las aclaraciones aprobadas, incluidas las combinaciones de categorías y editores. Confirma que los cambios siguen las instrucciones existentes del repositorio.
3. Revisa los resultados de lint, las pruebas unitarias, las pruebas E2E y la comprobación de tipos. Una comprobación omitida no cuenta como superada.
4. Resuelve los errores y vuelve a ejecutar las comprobaciones afectadas antes de aceptar la implementación. La configuración E2E de Playwright compila y sirve una vista previa y puede reutilizar un servidor local; asegúrate de que el servidor probado pertenece a este árbol de trabajo, no a una lección anterior.

## Explorar la nueva funcionalidad

El código parece correcto, pero ¿se ejecuta? Inicia la aplicación como antes y abre el sitio en un canvas de navegador.

1. Pide a Copilot que inicie la aplicación y abra la página en el canvas:

    ```plaintext
    Inicia la aplicación y ábrela en el canvas del navegador.
    ```

2. En unos instantes, se iniciará la aplicación y se abrirá una ventana del navegador dentro de la aplicación Copilot.
3. Selecciona una categoría y confirma que los resultados coinciden. Borra el filtro, selecciona un editor y confirma que los resultados coinciden con ese editor.
4. Combina los filtros de categoría y editor y compara los resultados con la incidencia y el plan aprobado. Si se admiten varias categorías, comprueba el comportamiento acordado para combinarlas.
5. Prueba una combinación sin juegos coincidentes y revisa el estado vacío. Borra o restablece los filtros y confirma que vuelve a aparecer la lista completa.
6. Cuando termines, pide a Copilot que detenga el servidor de desarrollo que ha iniciado para esta sesión:

    ```plaintext
    Detén el servidor de desarrollo y cierra el canvas del navegador.
    ```

## Conservar esta sesión para las lecciones siguientes

Todavía no crees ni combines la PR del filtrado. Conserva esta sesión, este árbol de trabajo y esta rama para las lecciones 4 a 8. A continuación, aplicarás instrucciones personalizadas al código de filtrado y añadirás mejoras a la habilidad y al agente de QA antes de crear y combinar la PR conjunta en la lección 8. Cuando hayas detenido el servidor, puedes dejar la sesión inactiva y volver a ella en la siguiente lección.

## Resumen y próximos pasos

Has utilizado distintos modos de agente para crear y revisar una funcionalidad. En esta lección:

- iniciaste una sesión de agente nueva a partir de una incidencia de GitHub.
- definiste los requisitos en el modo **Plan**.
- implementaste la funcionalidad con el modo **Autopilot**.
- revisaste el código.
- validaste manualmente la funcionalidad en un canvas de navegador.

A continuación, profundizarás en cómo se genera el código y cómo seguir las prácticas documentadas mediante [instrucciones personalizadas][next-lesson].

## Recursos

- [Trabajar con sesiones de agente en la aplicación GitHub Copilot][agent-sessions]

[next-lesson]: ../4-custom-instructions/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
