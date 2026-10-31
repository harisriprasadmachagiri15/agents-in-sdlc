---
title: "Lección 1 - Instalar la aplicación GitHub Copilot"
description: "Instala la aplicación GitHub Copilot, conecta el repositorio que has creado a partir de la plantilla, familiarízate con el espacio de trabajo y prueba un chat rápido."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La [**aplicación GitHub Copilot**][about-copilot-app] es una aplicación de escritorio para el desarrollo dirigido por agentes. Se basa en GitHub Copilot CLI y se integra de forma nativa con GitHub, por lo que los repositorios, las ramas y las canalizaciones de integración continua funcionan desde el primer momento. Está diseñada para flujos de trabajo en los que diriges varios agentes en paralelo —cada uno en su propio espacio de trabajo aislado— en lugar de hacer todo el trabajo por tu cuenta, y para automatizar tareas repetitivas. Ya tienes Node.js instalado y una copia del proyecto preparada. Ahora instalarás la aplicación y conectarás el repositorio.

En esta lección:

- instalarás la aplicación GitHub Copilot e iniciarás sesión.
- añadirás el proyecto a la aplicación desde su repositorio de GitHub.
- conocerás el espacio de trabajo, incluido el backlog que ha creado la plantilla.
- probarás un chat rápido para conocer la propia aplicación.

## Escenario

Tu equipo está adoptando agentes de inteligencia artificial (IA) para abordar un backlog cada vez mayor. La aplicación Copilot ofrece un único lugar desde el que dirigir ese trabajo: seleccionar incidencias, ejecutar agentes, revisar cambios y combinar solicitudes de incorporación de cambios. En esta lección instalarás y conectarás la aplicación y aprenderás a iniciar una conversación sobre el proyecto.

> [!NOTE]
> Se requiere un plan de Copilot válido: Copilot Free, Copilot Student o cualquier plan de pago (Pro, Pro+, Business o Enterprise). Si utilizas Copilot Business o Copilot Enterprise, el administrador debe mantener habilitada la directiva **GitHub Copilot app**.

## Instalar y configurar la aplicación GitHub Copilot

El primer paso para utilizar la aplicación GitHub Copilot es instalarla. Hay versiones para Windows, macOS y Linux. Instala la aplicación, autentícate y añade el repositorio de Tailspin Toys.

1. En un navegador, abre la [página de inicio de la aplicación GitHub Copilot][download-app].
2. Descarga la aplicación para tu plataforma e instálala siguiendo las instrucciones de la página.
3. Abre la aplicación después de instalarla.
4. Selecciona **Sign in to GitHub** y sigue las indicaciones para autenticarte. Si utilizas GitHub Enterprise Cloud con residencia de datos, selecciona **Use GitHub Enterprise** e introduce el nombre de host `*.ghe.com` cuando se te solicite. GitHub Enterprise Server no es compatible.
5. Si el proceso de incorporación te ofrece seleccionar un repositorio, selecciona el repositorio de Tailspin Toys que has creado a partir de la plantilla. También puedes omitir la selección y añadir el proyecto después.
6. Cuando se te solicite, elige un tema y termina el proceso de incorporación.
7. Busca el repositorio en **Sessions**, en la barra lateral. Si ya aparece en la lista, no hace falta que lo añadas otra vez.
8. Si el repositorio aún no está conectado, selecciona **+** junto a **Sessions** para crear una sesión y, después, selecciona **Add GitHub repository**. Especifica el propietario y el nombre del repositorio que anotaste en la lección de requisitos previos.
9. Confirma que el repositorio aparece en la barra lateral. En la disposición que se utiliza aquí, las carpetas de repositorio, como `tailspin-toys`, aparecen agrupadas en **Sessions**, con sus sesiones debajo de cada repositorio.

## Familiarizarse con el espacio de trabajo

Con el proyecto conectado, dedica un momento a conocer el espacio de trabajo. La aplicación organiza todo en varias áreas de la barra lateral:

- **New**: inicia una sesión de chat nueva con Copilot.
- **Pull requests**: muestra las solicitudes de incorporación de cambios mediante la integración nativa con GitHub. Desde aquí puedes explorarlas y filtrarlas, comprobar el estado de integración continua y revisarlas sin salir de la aplicación.
- **Issues**: muestra las incidencias de la misma forma. Desde aquí puedes explorarlas y filtrarlas e iniciar una sesión a partir de una incidencia.
- **Automations**: contiene tareas de agente guardadas que se ejecutan según una programación o bajo demanda. Son útiles para administrar listas de tareas, realizar tareas periódicas de mantenimiento del proyecto o delegar otras tareas tediosas. El resumen presenta las automatizaciones como próximo paso, no como otro ejercicio del taller.
- **Customize**: permite añadir funciones a la aplicación Copilot, como servidores del Protocolo de contexto de modelo (MCP), plugins, habilidades y otros componentes. Lo utilizarás para configurar Playwright MCP.
- **Chats**: contiene conversaciones breves para preguntas y lluvias de ideas que no necesitan una rama ni un espacio de trabajo propios. Esta sección aparece después de iniciar el primer chat. Si no está visible, selecciona **+** junto a **Sessions** para iniciar un chat.
- **Sessions**: agrupa las carpetas de los repositorios y sus sesiones de agente. Selecciona el nombre de un repositorio, como `tailspin-toys`, para abrir su vista, o selecciona una sesión para volver a la conversación. Cada sesión se ejecuta en su propio espacio de trabajo aislado, de modo que puedes ejecutar varias a la vez sin que sus cambios entren en conflicto. Iniciarás tu primera sesión al añadir las valoraciones con estrellas.

A lo largo del taller, explorarás el espacio de trabajo.

> [!TIP]
> Si tienes dudas, pregunta a Copilot. Si no sabes cómo hacer algo o si es posible, puedes preguntarle y te ayudará a orientarte.

### Localizar el backlog inicial

Probablemente todos los proyectos tienen trabajo pendiente, y Tailspin Toys no es una excepción. Explora el backlog que se creó al crear el repositorio a partir de la plantilla.

1. En **Sessions**, en la barra lateral, selecciona la carpeta del repositorio de Tailspin Toys, no una sesión individual. Después, selecciona **Issues** en la navegación superior del repositorio.
2. Busca estas incidencias por su título, no por el número:

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. Selecciona una incidencia para leer los detalles. Cada incidencia también permite iniciar una sesión de agente. Después de hacer un primer cambio rápido, partirás de la incidencia de filtrado.

> [!NOTE]
> Las listas de **Issues** y **Pull requests** solo muestran automáticamente los elementos de los repositorios que has añadido a la aplicación Copilot. Para ver elementos de trabajo de otros repositorios, añádelos a la aplicación.

## Probar un chat rápido

Una buena forma de familiarizarse con la aplicación es utilizarla para conocer la propia aplicación. Un **chat rápido** es una herramienta adecuada para ello. Permite hacer una pregunta o plantear ideas sin crear una rama ni un árbol de trabajo; es ideal para consultas breves que no requieren una sesión.

1. En la barra lateral, selecciona **+** junto a **Chats** para abrir un chat nuevo. Si **Chats** aún no está visible, selecciona **+** junto a **Sessions** para iniciar el primer chat; después aparecerá **Chats**.
2. Pregunta a la aplicación cómo funcionan sus sesiones:

   ```plaintext
   ¿Cómo utiliza árboles de trabajo la aplicación GitHub Copilot?
   ```

3. Lee la respuesta en la vista de conversación. Verás que cada sesión se ejecuta en su propio árbol de trabajo de Git aislado, lo que permite ejecutar varios agentes en paralelo sin que sus cambios entren en conflicto. Puedes continuar la conversación o iniciar un chat nuevo cuando quieras.

## Resumen y próximos pasos

¡Enhorabuena! Has instalado la aplicación GitHub Copilot, conectado el proyecto y explorado el espacio de trabajo. Has aprendido a:

- instalar la aplicación e iniciar sesión en GitHub.
- añadir un proyecto desde su repositorio de GitHub.
- familiarizarte con el espacio de trabajo y localizar el backlog inicial en **Issues**.
- utilizar un chat rápido para formular una pregunta breve y puntual.

A continuación, [iniciarás tu primera sesión de agente][next-lesson] y la utilizarás para mostrar valoraciones con estrellas en las tarjetas de los juegos.

## Recursos

- [Acerca de la aplicación GitHub Copilot][about-copilot-app]
- [Introducción a la aplicación GitHub Copilot][getting-started]
- [Trabajar con sesiones de agente en la aplicación GitHub Copilot][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
