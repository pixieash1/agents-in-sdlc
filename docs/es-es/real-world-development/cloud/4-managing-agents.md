---
title: "Ejercicio 4 - Supervisar y administrar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has pedido al agente de GitHub Copilot en la nube que aborde tres tareas distintas para mejorar la experiencia de usuario y añadir funcionalidades. Aunque el agente trabaja de forma asíncrona y autónoma, también es importante supervisar estas tareas.

Hay varias herramientas para administrar las tareas asignadas al agente en la nube, incluida la [página de agentes][agents-page] de GitHub.com. Desde este centro puedes ver las tareas de agente con solicitudes de incorporación de cambios abiertas, explorar las acciones realizadas e incluso orientar una sesión que sigue en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube;
- dirigirás una sesión en curso y le pedirás funcionalidad adicional.

## Escenario

Después de asignar al agente la creación de un modo de alto contraste, el equipo decide añadir también un modo claro. Como ya se están actualizando los estilos y se está añadiendo la funcionalidad de alternancia, es lógico incluirlo. Dirigirás el trabajo del agente para que añada ambos modos.

## Revisa las tareas del agente en la nube

Veamos el estado de las tareas asignadas al agente de GitHub Copilot en la nube.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa las tareas en el panel principal y en el panel izquierdo. Deberías ver las tareas que asignaste a Copilot, entre ellas:
   - actualizar la documentación del código;
   - generar API para modificar juegos;
   - añadir un modo de alto contraste al sitio.
3. Selecciona una tarea en curso y revisa las acciones que ha realizado Copilot. Pueden incluir:
   - obtener el código del repositorio;
   - configurar el entorno de trabajo de Copilot;
   - configurar servidores MCP;
   - realizar pasos para completar la tarea.

> [!NOTE]
> La lista exacta de pasos depende del estado del trabajo de Copilot y del enfoque que haya utilizado.

4. Observa también el panel de solicitud de incorporación de cambios (PR) de la derecha. Desde ahí puedes ver la PR y los archivos modificados para supervisar el trabajo.

## Dirige el agente en la nube

Ahora que has visto las tareas activas, pídele a Copilot que añada el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto depende del nombre que haya utilizado Copilot y del estado de la tarea.

    ![Sesión de accesibilidad en mission control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos hasta que indique que se ha completado la configuración y ha empezado el trabajo.
3. En el diálogo **Steer active session while Copilot is working**, escribe este prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Captura de la tarea del agente en la nube en la página de agentes, con el diálogo para dirigir la sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa que Copilot lo confirma y lo incorpora al flujo de trabajo.

## Deja que Copilot trabaje

Copilot incorporará la nueva solicitud al flujo después de terminar el paso que estaba ejecutando cuando enviaste el mensaje.

Como el trabajo puede tardar varios minutos, puedes aprovechar para repasar lo aprendido hasta ahora.

## Resumen y siguientes pasos

Has explorado la página de agentes de Copilot, el centro para supervisar y orientar las tareas del agente en la nube. Puedes realizar un seguimiento de las tareas activas y completadas, revisar el trabajo y redirigir las sesiones en curso para ajustar el alcance u ofrecer más indicaciones.

En esta lección:

- has explorado mission control y la página de agentes para supervisar las tareas del agente en la nube;
- has redirigido una sesión en curso para solicitar funcionalidad adicional.

Mientras Copilot termina las funcionalidades de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Más información sobre los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
