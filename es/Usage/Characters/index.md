---
order: 100
icon: person-fill
route: /usage/characters/
---

# Personajes

Los personajes son identidades de IA que puedes crear y administrar para moldear el papel de la IA en la conversación. Cada personaje tiene un nombre, personalidad e historial de conversación. Puedes crear tantos personajes como desees y cambiar entre ellos en cualquier momento.

Los personajes se pueden usar en chats individuales, o agregar múltiples personajes a un chat grupal para que interactúen entre sí.

## Panel de Gestión de Personajes

Abre el panel <i class="fa-solid fa-address-card"></i> **Personajes** desde la barra de navegación para acceder a la lista de personajes. Haz clic en un personaje o grupo para chatear con ellos o editarlos, o elige <i class="fa-solid fa-user-plus"></i> **Crear Nuevo Personaje** para agregar un nuevo personaje.

### Controles del Panel

* <i class="fa-solid fa-lock"></i> **Pin Panel**: Mantener el panel abierto mientras interactúas
* <i class="fa-solid fa-list-ul"></i> **Lista de Personajes**: Volver a la vista de lista de personajes
* **HotSwap Bar**: Acceso rápido a personajes favoritos

### Lista de Personajes

* <i class="fa-solid fa-user-plus"></i> **Crear Nuevo Personaje**: Agregar un nuevo personaje
* <i class="fa-solid fa-file-import"></i> **Importar Personaje**: Cargar personaje desde archivo
* <i class="fa-solid fa-cloud-arrow-down"></i> **Importación Externa**: Importar desde URL
* <i class="fa-solid fa-users-gear"></i> **Crear Grupo**: Iniciar un nuevo chat grupal

#### Buscar y Ordenar

* **Barra de Búsqueda**: Filtrar personajes por nombre o atributos
* **Menú Ordenar**: Múltiples opciones de ordenamiento:
    - Alfabético (A-Z, Z-A)
    - Cronológico (Más Reciente, Más Antiguo)
    - Basado en uso (Reciente, Más/Menos chats)
    - Basado en tamaño (Más/Menos tokens)
    - Especial (Favoritos, Aleatorio)

#### Filtrar Personajes por Tipo o Etiqueta

* <i class="fa-solid fa-star"></i> **Filtro Favoritos**: Mostrar personajes favoritos
* <i class="fa-solid fa-users"></i> **Filtro de Grupos**: Mostrar solo chats grupales
* <i class="fa-solid fa-folder-plus"></i> **Etiquetas como Carpetas**: Organizar por jerarquía de etiquetas
* <i class="fa-solid fa-gear"></i> **Gestionar Etiquetas**: [Configuración de etiquetas](/Usage/Characters/Tags.md)
* <i class="fa-solid fa-tags"></i> **Lista de Etiquetas**: Ver todas las etiquetas disponibles
* <i class="fa-solid fa-filter-circle-xmark"></i> **Limpiar Filtros**: Restablecer todos los filtros

### Panel de Creación/Edición de Personaje

* **Imagen de Avatar**: Cargar y previsualizar foto de perfil del personaje
* **Recuento de Tokens**: [Uso de tokens](characterdesign.md#character-tokens) del personaje
* <i class="fa-solid fa-ranking-star"></i> **Estadísticas**: Historial de chat y estadísticas de uso
* [Gestión de etiquetas](/Usage/Characters/Tags.md)

#### Acciones Rápidas

- <i class="fa-solid fa-star"></i> Alternar favorito
- <i class="fa-solid fa-book"></i> Definiciones avanzadas
- <i class="fa-solid fa-globe"></i> Lore del personaje
- <i class="fa-solid fa-passport"></i> Lore del chat: vincular el chat a una [World Info](/Usage/worldinfo.md)
- <i class="fa-solid fa-file-export"></i> Exportar personaje
- <i class="fa-solid fa-clone"></i> Duplicar
- <i class="fa-solid fa-skull"></i> Eliminar

#### Opciones Extendidas

* Vinculación de World Info
* Importación de lore de tarjeta
* Anulación de escenario
* Conversión de persona
* Cambiar nombre del personaje
* Vinculación de fuente
* Reemplazar/Actualizar
* Importación de etiqueta
* Vista de galería

#### Campos de Contenido

* **[Descripción del Personaje](characterdesign.md#character-description)**: Resumen breve del personaje
* **[Primer Mensaje](characterdesign.md#first-message)**: Saludo inicial o indicación al iniciar un nuevo chat
* **Saludos Alternativos**: Define múltiples primeros mensajes entre los que puedes cambiar al iniciar un chat

### Panel de Definiciones Avanzadas

Haz clic en el botón <i class="fa-solid fa-book"></i> **Definiciones Avanzadas** para acceder a la configuración extendida del personaje.

#### Anulaciones de Indicación (Chat Completion/Instruct Mode)

* **Indicación Principal**: Reemplaza la [indicación principal/sistema predeterminada](/Usage/Prompts/index.md#main-prompt-system-prompt), puede usar el marcador de posición \{\{original\}\} para incluir la indicación original
* **Instrucciones Post-Historial**: Anula las [instrucciones post-historial predeterminadas](/Usage/Prompts/index.md#post-history-instructions)

#### Metadatos del Creador

Información no relacionada con indicaciones sobre el personaje:

- Nombre/contacto del creador
- Versión del personaje
- Notas del creador
- Lista de etiquetas incrustadas

#### Personalidad del Personaje

* **[Resumen de Personalidad](characterdesign.md#personality-summary)**: Descripción general breve de los rasgos del personaje
* **[Escenario](characterdesign.md#scenario)**: Contexto y circunstancias del diálogo
* **Nota del Personaje**: Mensaje personalizado con profundidad seleccionable y rol de mensaje (también ver [Nota del Autor](/Usage/Characters/Author's-Note.md))
* **Locuacidad** (Chats Grupales): Deslizador para Tímido → Normal → Charlatán
* **Mensajes de Ejemplo**: Ejemplos del estilo de escritura del personaje

### Gestión de Chats Grupales

Si esto es un chat grupal, puedes administrar los miembros y la configuración del grupo desde este panel.

Consulta [Chats Grupales](/Usage/Characters/groupchats.md) para más detalles.
