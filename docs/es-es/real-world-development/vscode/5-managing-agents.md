---
title: "Ejercicio 5 - Supervisar y administrar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando pones GitHub Copilot en modo agente, trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Como el trabajo se realiza en tu equipo y en tiempo real, Copilot Chat en VS Code muestra cada llamada a una herramienta y cada edición a medida que se producen. Puedes revisar las diferencias en línea, aceptar o rechazar cada cambio y orientar la conversación con mensajes de seguimiento para ajustar o ampliar el trabajo sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás los cambios propuestos en los archivos.
- orientarás la sesión del agente para que añada un conmutador de modo claro junto al de alto contraste.

## Escenario

El agente personalizado de accesibilidad que utilizaste en el [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño solicita además un conmutador de modo claro para las personas a quienes les resulte demasiado oscuro el tema predeterminado. En lugar de empezar una conversación nueva, ampliarás el trabajo de la sesión actual.

## Supervisar el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada edición aparece en el chat con sus diferencias y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para observar las llamadas a herramientas. Verás lecturas y ediciones de archivos y los comandos de terminal que solicite ejecutar.
3. Cuando el agente proponga una edición, aparecerán las diferencias en línea:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para revertir el cambio y permitir que el agente vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed** de la parte superior del chat para ver todos los archivos modificados durante esta sesión.

> [!NOTE]
> Si una llamada a una herramienta solicita permiso —por ejemplo, para ejecutar un comando de terminal—, el agente se detendrá y mostrará una solicitud **Allow**. Aprueba únicamente los comandos que te parezcan adecuados.

5. Espera a que el agente termine de implementar el modo de alto contraste. Deberías ver un mensaje final que resuma los cambios.

## Pedir al agente que añada el modo claro

Ahora que ya está implementado el modo de alto contraste, ampliarás la misma conversación para añadir un conmutador de modo claro. Como continuarás en la misma sesión de chat, el agente conservará el contexto del trabajo anterior, incluidos los componentes modificados, el enfoque de estilo y el mecanismo de almacenamiento local.

1. En el campo de entrada de Copilot Chat, envía este mensaje:

   ```plaintext
   ¡Buen trabajo! Ahora añade un conmutador de modo claro junto al de alto contraste. El modo claro debe seguir el mismo patrón: se conserva mediante el almacenamiento local y está disponible en la misma zona de la interfaz. El usuario debe poder activar de forma independiente el modo de alto contraste y el modo claro.
   ```

2. El agente continuará desde donde lo dejó, revisará los cambios anteriores y propondrá los cambios necesarios para añadir el modo claro.
3. Revisa cada cambio nuevo a medida que aparezca, como hiciste con el modo de alto contraste. Acepta o rechaza cada diferencia con **Keep** o **Undo**.
4. Si el agente sigue un enfoque que no quieres —por ejemplo, intenta reorganizar los conmutadores de una forma que no te convence—, deténlo con el botón **Stop** junto al campo de entrada del chat y envía un mensaje para aclararlo.

> [!TIP]
> Dirigir una sesión en curso es más rápido que empezar de nuevo. Utiliza mensajes de seguimiento para pedir ajustes («simplifica las hojas de estilo en cascada (CSS) y usa variables CSS en lugar de duplicar selectores»), corregir el enfoque («no añadas un tercer conmutador; integra el modo claro en el panel de configuración existente») o ampliar el alcance («añade también métodos abreviados de teclado para los dos conmutadores»).

## Revisar el conjunto de cambios

Antes de confirmar el trabajo, revisa rápidamente todos los archivos que haya modificado el agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver cambios en componentes Astro, estilos y pruebas relacionadas.
3. Abre algunos archivos modificados y examina las diferencias. Confirma que se aplican los patrones de accesibilidad del agente personalizado: atributos de aplicaciones de Internet enriquecidas accesibles (ARIA), navegación con teclado, lenguaje de marcado de hipertexto (HTML) semántico y persistencia mediante almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en la [siguiente lección][next-lesson].

## Resumen y próximos pasos

Has utilizado el modo agente de Copilot Chat para supervisar y dirigir una sesión en curso: has revisado cada edición al aparecer, has aceptado o rechazado los cambios y has enviado mensajes de seguimiento para ampliar el trabajo sin perder el contexto.

En esta lección:

- supervisaste el trabajo del agente en tiempo real.
- aceptaste y rechazaste ediciones individuales.
- dirigiste una sesión en curso mediante mensajes de seguimiento.

En la [siguiente lección][next-lesson] publicarás el trabajo de accesibilidad como solicitud de incorporación de cambios y revisarás los cambios que ha producido el agente personalizado local durante el taller.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Usar el modo agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
