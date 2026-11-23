---
title: "Ejercicio 5 - Supervisar y administrar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando utilizas el modo de agente de GitHub Copilot, trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Como el trabajo se realiza en tu ordenador en tiempo real, Copilot Chat en VS Code muestra cada llamada a herramienta y cada edición de archivo. Puedes revisar los diffs en línea, aceptar o rechazar los cambios y orientar la conversación con prompts de seguimiento sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad;
- revisarás y aceptarás los cambios propuestos en los archivos;
- dirigirás la sesión para añadir un control de modo claro junto al de alto contraste.

## Escenario

El agente personalizado de accesibilidad que utilizaste en el [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño solicita además un modo claro para quienes consideran que el tema predeterminado es demasiado oscuro. En vez de iniciar una conversación desde cero, dirigirás la sesión actual para ampliar el trabajo.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada edición aparece en el chat con un diff y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja, desplázate por el chat para ver cada llamada a herramienta, lectura y edición de archivos, así como los comandos de terminal que solicita ejecutar.
3. Cuando el agente proponga una edición, el diff aparecerá en línea:
   - selecciona **Keep** para aceptar el cambio;
   - selecciona **Undo** para revertirlo y pedir al agente que vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed** en la parte superior del chat para consultar todos los archivos modificados durante la sesión.

> [!NOTE]
> Si una llamada a herramienta necesita permiso —por ejemplo, para ejecutar un comando de terminal—, el agente se detendrá y mostrará una solicitud **Allow**. Autoriza solo los comandos que te parezcan adecuados.

5. Espera a que termine de implementar el modo de alto contraste. Al final debería aparecer un resumen de los cambios.

## Pide al agente que añada el modo claro

Ahora que está implementado el modo de alto contraste, ampliarás la misma conversación para añadir un control de modo claro. Como continuarás en la misma sesión, el agente conservará el contexto de su trabajo: los componentes que modificó, el enfoque de estilos y el uso de almacenamiento local.

1. Envía este prompt de seguimiento desde el cuadro de Copilot Chat:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. El agente revisará los cambios anteriores y propondrá cómo añadir el modo claro.
3. Revisa cada cambio nuevo cuando aparezca y selecciona **Keep** o **Undo**, como hiciste con el modo de alto contraste.
4. Si el agente sigue un enfoque que no quieres —por ejemplo, intenta reorganizar de una forma que no te convence la interfaz de los controles—, deténlo con el botón **Stop** junto al cuadro de chat y envía una aclaración.

> [!TIP]
> Dirigir una sesión en curso es más rápido que empezar de nuevo. Utiliza prompts de seguimiento para solicitar ajustes («simplifica el CSS y usa variables CSS en lugar de duplicar selectores»), corregir el enfoque («no añadas un tercer control; integra el modo claro en el panel de configuración existente») o ampliar el alcance («añade también métodos abreviados de teclado para ambos controles»).

## Revisa los cambios

Antes de confirmar el trabajo, revisa rápidamente todos los archivos modificados por el agente.

1. Abre la vista **Source Control** en VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver cambios en los componentes Astro, los estilos y las pruebas relacionadas.
3. Abre algunos archivos y revisa sus diffs. Confirma que se aplican los patrones de accesibilidad del agente personalizado: atributos ARIA, navegación con teclado, HTML semántico y persistencia en el almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la siguiente lección][next-lesson].

## Resumen y siguientes pasos

Has utilizado el modo de agente de Copilot Chat para supervisar y dirigir una sesión en curso: has revisado cada edición, aceptado o rechazado cambios y enviado prompts de seguimiento para ampliar el trabajo sin perder contexto.

En este ejercicio:

- has supervisado el trabajo de un agente en tiempo real;
- has aceptado y rechazado ediciones de archivos;
- has dirigido una sesión en curso con prompts de seguimiento.

En la [siguiente lección][next-lesson] publicarás el trabajo de accesibilidad en una solicitud de incorporación de cambios y revisarás los cambios que haya producido el agente local durante el taller.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Uso del modo de agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
