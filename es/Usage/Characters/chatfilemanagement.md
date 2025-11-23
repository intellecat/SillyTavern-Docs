---
order: 80
route: /usage/core-concepts/chatfilemanagement/
---

# Gestión de Archivos de Chat

Esta página describe las formas en que puede gestionar sus archivos de chat de IA.

!!!info Nota
Algunas de estas opciones están disponibles en el diálogo "Manage chat files" que se abre desde el menú de opciones de la esquina inferior izquierda.
!!!

## Chats Individuales vs Chats Grupales

La forma más simple de usar una tarjeta de personaje es un chat individual; simplemente haga clic en su tarjeta y comience a chatear.

Una vez que tenga algunas tarjetas de personaje, también puede usar el botón "Create New Chat Group" para crear un [chat grupal](/Usage/Characters/groupchats.md) que incluya múltiples personajes que luego interactuarán entre sí y con usted.

## Importar Chats

**Importar chats de Character.AI a SillyTavern.**

Para importar chats y bots de Character.AI, use la extensión del navegador CAI Tools: [https://github.com/irsat000/CAI-Tools](https://github.com/irsat000/CAI-Tools).

Otros programas y herramientas de los que puede importar chats incluyen:

* TavernAI (original): <https://github.com/TavernAI/TavernAI>
* Text Generation WebUI (oobabooga): <https://github.com/oobabooga/text-generation-webui>
* Agnai: <https://github.com/agnaistic/agnai>
* KoboldAI Lite: <https://github.com/LostRuins/lite.koboldai.net>
* RisuAI: <https://github.com/kwaroran/RisuAI>

## Exportar como .jsonl

Al hacer clic en "Manage chat files", cada entrada en la lista de archivos de chat tendrá un botón para exportarlo en un formato que luego puede ser reimportado. Use esto para compartir o migrar chats incluyendo todos sus metadatos (pero excluyendo imágenes y archivos adjuntos).

Si le preocupa la privacidad, asegúrese de inspeccionar el archivo JSONL exportado y eliminar cualquier cosa que no desee compartir.

## Exportar como .txt

También puede exportar una versión simplificada de solo texto con el botón "Download chat as plain text document". ¡No se puede reimportar nuevamente ya que pierde metadatos importantes!

## Puntos de Control

Los "Checkpoints" son clones del chat actual, en el sentido de que copian todos los mensajes del chat dado hasta cierto punto, y almacenan un enlace a la fuente (por nombre de archivo de chat).

Desde el botón de tres puntos a la derecha de cada mensaje de chat, tiene dos formas de crear puntos de control:

* "Create Branch" clonará el chat actual hasta ese mensaje y cambiará a él
* "Create Checkpoint" clonará el chat actual hasta ese mensaje, pedirá un nombre y lo creará pero NO cambiará a él

Puede pensar en ellos aproximadamente como "abrir enlace en nueva pestaña" y "abrir enlace en nueva pestaña en segundo plano" en un navegador.

Puede volver al chat principal desde un punto de control haciendo clic en el botón de menú de hamburguesa a la izquierda del cuadro de texto del mensaje y luego haciendo clic en "Back to parent chat".

## Renombrar Chat

Por defecto, los archivos de chat se nombran con la fecha y hora en que se iniciaron.

Puede cambiar esto haciendo clic en el icono de lápiz e ingresando un nuevo nombre.

Tenga en cuenta que esto romperá los enlaces a ese chat desde los puntos de control (ya que están vinculados por nombre de archivo de chat).
