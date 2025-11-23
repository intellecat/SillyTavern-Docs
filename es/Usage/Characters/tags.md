---
order: 60
route: /usage/core-concepts/tags/
---

# Etiquetas

Character cards and groups can be assigned zero or more tags. They are useful to organize quickly growing collections by themes, quality, provenance or whatever you like.

## Etiquetado

There are several ways to add or remove tags to a character card:

- Importar etiquetas integradas durante la importación.
- Abrir una tarjeta desde el panel Character Management. Desde allí podrá asignar etiquetas a una tarjeta de personaje.
- Etiquetado masivo.

Para hacer etiquetado masivo, haga clic en el botón "Bulk edit characters" (icono de lápiz), seleccione las tarjetas que desea etiquetar, haga clic derecho en cualquiera de ellas y luego haga clic en "Tag" en el menú contextual.

!!!info Nota
Tenga en cuenta que los grupos no se pueden etiquetar de forma masiva.
!!!

Desde esta pantalla podrá:

- Agregar o eliminar etiquetas usando el cuadro combinado.
- Eliminar todas las etiquetas de las tarjetas seleccionadas ("All").
- Eliminar la intersección de etiquetas entre todas las tarjetas seleccionadas de esas tarjetas ("Mutual").
- Importar (crear localmente) todas las etiquetas almacenadas en la tarjeta de personaje, en caso de que la haya importado ("Import All").
- Importar (crear localmente) etiquetas almacenadas en la tarjeta de personaje que también existen localmente con nombres coincidentes ("Import Existing").

## Administración

Para ver y administrar todas las etiquetas existentes, abra el panel Character Management y luego haga clic en el botón "Manage tags" (icono de engranaje).

Puede hacer copia de seguridad y restaurar toda la información aquí (lista de etiquetas, asignaciones de etiquetas a tarjetas, colores, configuración de carpetas, etc.) usando los botones en la esquina superior derecha.

Puede usar los botones de agarre en la izquierda para reordenar las etiquetas como aparecerán en el filtro de etiquetas en Character Management.

!!!warning Advertencia
El archivo JSON de copia de seguridad de etiquetas no está destinado a compartirse con otros, ya que contiene información específica de su instancia, ¡como nombres de entidades internas!
!!!

## Importar etiquetas al importar tarjetas de personaje

Al importar tarjetas de personaje externas de imágenes descargadas (o desde el botón "Import content from external URL"), se le pedirá que opcionalmente importe las etiquetas que contiene. No son necesarias para que la tarjeta funcione; las etiquetas son simplemente organizativas.

Las etiquetas de la tarjeta incrustada se almacenan en la sección "Creator's Metadata" del menú "Advanced Definitions" del editor de personajes. Si desea proponer algunas etiquetas a otros usuarios que importarían ese personaje, complete el campo "Tags to Embed" con una lista de etiquetas separadas por comas.

!!!info Nota
Este mensaje emergente aparecerá solo si una opción de User Settings "Import Card Tags" está configurada en "Ask".
!!!

En el mensaje emergente "Import tags for CHARACTER NAME" que se abre, verá una lista de etiquetas existentes (que ya tenía localmente con un nombre coincidente) y etiquetas nuevas (que no tenía localmente).

Puede:

- Ajustar las listas según sea necesario y luego presionar "Import": las etiquetas existentes restantes se agregarán a la tarjeta de personaje importada, y las etiquetas nuevas restantes se crearán localmente y luego se agregarán a la tarjeta.
- O simplemente presione "Import none" para ignorar las etiquetas contenidas en la tarjeta de personaje e importar SOLO la tarjeta.
- O "Import All" como acceso directo para importar todas las etiquetas encontradas en la tarjeta de personaje (NOTA: incluidas las que recortó de las listas anteriores; use el botón "Import" si lo hizo).
- O "Import Existing" como acceso directo para importar solo las etiquetas que existían localmente con un nombre coincidente.

## Filtrado de tarjetas de personaje

Después de crear etiquetas, las verá en una fila en el panel Character Management. Puede hacer clic en estas para cambiar el estado del filtro de etiquetas; en orden:

- Un clic mostrará las tarjetas etiquetadas con esta etiqueta.
- Otro clic para mostrar solo las tarjetas NO etiquetadas con esta etiqueta.
- Otro clic para restablecer el filtrado por esta etiqueta.

Puede filtrar por cualquier número de etiquetas al mismo tiempo.

## Etiquetas como carpetas

!!!info Nota
Para usar esta funcionalidad, primero debe estar habilitada en User Settings, bajo la columna UI Theme. El estado de este conmutador también se guarda con el tema de interfaz de usuario.
!!!

Desde el botón "Manage tags" (icono de engranaje), cada entrada de etiqueta tiene un botón de alternancia de varios estados para cambiar entre estos modos de etiquetas como carpeta (llamados "bogus folder" en el código):

- un clic para convertir esta etiqueta en una "carpeta abierta". Aparecerá como una entrada virtual en la lista de tarjetas; hacer clic en ella solo mostrará las tarjetas con esa etiqueta
- otro clic para convertir esta etiqueta en una "carpeta cerrada". Como se indicó anteriormente, pero las tarjetas etiquetadas con esta etiqueta no aparecerán de forma predeterminada; tendrá que hacer clic en la carpeta para verlas.
- otro clic para restablecer el estado de etiqueta como carpeta para esta etiqueta.
