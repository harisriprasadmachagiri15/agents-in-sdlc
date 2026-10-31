---
title: "Lección 8 - Crear y combinar la solicitud de incorporación de cambios (PR) de la funcionalidad"
description: "Revisa conjuntamente el filtrado, las instrucciones, la actualización de la habilidad, el perfil de aseguramiento de la calidad (QA) y las pruebas; después, crea una PR y utiliza Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La implementación del filtrado, las actualizaciones de instrucciones, la actualización de la habilidad, el perfil de aseguramiento de la calidad (QA) y las pruebas están guardados en una rama. Es el momento de revisarlos en conjunto y abrir una solicitud de incorporación de cambios. Combinaste manualmente la PR de las valoraciones con estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las actualizaciones de instrucciones, la actualización de la habilidad y el agente de QA en varias PR. Para simplificar el taller, has mantenido todo el flujo de filtrado y calidad en una sesión y una rama, y lo incluirás en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación.
- inspeccionarás todos los cambios de la PR de la funcionalidad y las pruebas de verificación.
- autorizarás Agent Merge solo después de revisarlos y confirmarás que la PR se ha combinado.

## Escenario

Durante el flujo del filtrado, has utilizado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere automatizar el trabajo pendiente de la PR y mantener bajo el control del desarrollador la autorización para combinarla.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de incorporación de cambios en la aplicación GitHub Copilot. Al activarlo, la sesión de la aplicación lee la PR y resuelve los bloqueos: corrige las comprobaciones de integración continua (CI) fallidas, responde a comentarios de revisión y vuelve a aplicar la base cuando hace falta. Después, combina la solicitud en cuanto GitHub lo permite. El proceso se ejecuta en segundo plano, continúa aunque reinicies la aplicación y se desactiva cuando la PR se ha combinado.

Hasta ahora has seleccionado **Merge pull request** manualmente. Agent Merge puede encargarse de esa tarea, pero su capacidad para editar código y combinar la solicitud sigue requiriendo tu autorización explícita. Revisa las acciones permitidas y el trabajo antes de conceder permiso para combinar.

## Usar Agent Merge para gestionar la PR

Ya has creado y revisado todo el código. Ahora dejarás que Agent Merge gestione el proceso de la PR.

1. Utiliza el selector de agentes para seleccionar **Default agent**.
2. Selecciona el menú desplegable junto a **Create PR**.
3. Selecciona **Agent Merge**. El botón cambiará a **Agent Merge**.
4. Selecciona el botón **Agent Merge** para iniciar el proceso. Elegir la opción del menú desplegable no lo inicia por sí solo.

Se iniciará el proceso de Agent Merge. Hará lo siguiente:

- Crear la solicitud de incorporación de cambios con un título y una descripción.
- Si has iniciado la sesión desde una incidencia, incluir una referencia a la incidencia relacionada en el cuerpo de la descripción.
- Volver a aplicar la base o resolver posibles conflictos con la rama de destino.
- Supervisar el proceso de integración continua para comprobar que se superan todas las comprobaciones.
- Supervisar los comentarios de otros desarrolladores y los de la revisión de código de Copilot en la PR, y actualizar el código para resolverlos.
- Combinar automáticamente la PR cuando todo termine correctamente, si así se lo indicas.

Puedes pedir a Agent Merge que combine la PR cuando se hayan superado todas las comprobaciones.

5. Revisa las diferencias de la PR y las pruebas de verificación antes de conceder permiso para combinarla.
6. Selecciona el menú desplegable junto a **Agent Merge**.
7. Comprueba que **Merge pull request** esté marcado.
8. Pide explícitamente al agente que combine la solicitud:

    ```plaintext
    Combina la PR cuando se hayan superado todas las comprobaciones obligatorias y se hayan obtenido las aprobaciones. No omitas las protecciones del repositorio.
    ```

9. Sigue el proceso y resuelve los bloqueos que indique el agente. Confirma que el estado de la PR es **Merged** antes de continuar; tener habilitado el permiso de combinación o un agente en ejecución no confirma que la PR se haya combinado.

> [!IMPORTANT]
> Agent Merge no omite las protecciones del repositorio ni los permisos que falten. Resuelve esos bloqueos antes de continuar.

## Resumen y próximos pasos

Has automatizado varias partes del proceso de desarrollo: la generación de código, las pruebas y la validación, y ahora el proceso de la solicitud de incorporación de cambios. En esta lección:

- aprendiste qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación.
- inspeccionaste todos los cambios de la PR de la funcionalidad y las pruebas de verificación.
- autorizaste Agent Merge solo después de revisarlos y confirmaste que la PR se había combinado.

A continuación, [utilizarás un canvas existente y crearás uno de clasificación][next-lesson] para explorar una forma más completa de examinar, planificar y visualizar el trabajo con el agente.

## Recursos

- [Administrar incidencias y solicitudes de incorporación de cambios con la aplicación GitHub Copilot][managing-issues-prs]
- [Acerca de la aplicación GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
