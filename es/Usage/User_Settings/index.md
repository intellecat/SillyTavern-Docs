---
order: 120
icon: gear
route: /usage/user-settings/
---

# Configuración de Usuario


:::callout
**[Personalización de Interfaz](uicustomization.md)**

Cambia el tema, la apariencia y la sensación de la interfaz de chat según tus preferencias.
:::



:::callout
**[Modo Visual Novel](Visual-Novel.md)**

Chatea con personajes con sprites, como en novelas visuales como Doki Doki Literature Club y otros juegos VN famosos.
:::


## Configuración General

Estas son las configuraciones principales que afectan tu experiencia general de SillyTavern.

### Idioma de la Interfaz

La interfaz de usuario de SillyTavern está disponible en varios idiomas. El selector de idioma proporciona estas opciones:
* **Predeterminado**: Usa tu idioma del sistema si está disponible
* **English**: Fuerza la interfaz en English independientemente de la configuración del sistema
* Otros idiomas disponibles a través del menú desplegable

Nota: Esta configuración solo afecta al texto de la interfaz de usuario. Para la traducción de conversaciones con IA, utiliza la extensión [Traducción de Chat](../../extensions/Translation.md).

### Versión del Software

Tu versión actual de SillyTavern se muestra en la esquina superior derecha. Esta información es esencial para:
* Solucionar problemas
* Garantizar compatibilidad con extensiones
* Determinar si hay actualizaciones disponibles

Para actualizar SillyTavern a la última versión, consulta la documentación [Actualizar](/Installation/Updating).

### Gestión de Cuentas

Controla tu cuenta de usuario de SillyTavern, respalda tu configuración y datos de usuario, y gestiona roles de usuario y permisos en [modo multiusuario](/Administration/multi-user.md).

#### <i class="fa-fw fa-solid fa-user-shield"></i> Cuenta

En el diálogo Cuenta, puedes ver y editar tu información de perfil, cambiar tu contraseña y gestionar la configuración de tu cuenta.

**Información de Perfil**

* Nombre para mostrar (editable mediante ícono de lápiz)
* Avatar de usuario (también se puede cambiar usando [Personas](/Usage/personas.md))
* Identificador de cuenta
* Rol de usuario
* Fecha de creación de cuenta
* Estado de contraseña (ícono bloqueado/desbloqueado indica protección)

**Acciones de Cuenta**

* **Snapshots de Configuración**: Crea, gestiona y restaura copias de seguridad de tu configuración de usuario
* **Descargar Respaldo**: Exporta un respaldo completo de todos tus datos de usuario
* **Cambiar Contraseña**: Actualiza las credenciales de seguridad de tu cuenta

**Zona de Peligro**

Operaciones críticas de cuenta que deben usarse con precaución:
* **Restablecer Configuración**: Restaura toda la configuración a los valores predeterminados de fábrica
* **Restablecer Todo**: Eliminación completa de cuenta y reinicio de fábrica

#### <i class="fa-fw fa-solid fa-user-tie"></i> Panel de Administración

!!! Se aplica a: [modo multiusuario](/Administration/multi-user.md)

Las características de múltiples cuentas requieren que `enableUserAccounts` esté establecido en true en config.yaml.
!!!

Selecciona **Gestionar Usuarios** para ver y gestionar cuentas de usuario existentes.

##### Perfil de Usuario

- Gestión de avatares personalizados (cargar/eliminar)
- Nombre para mostrar e identificador
- Información de rol y estado
- Fecha de creación de cuenta
- Estado de protección de contraseña

##### Controles de Cuenta

- <i class="fa-fw fa-solid fa-pencil"></i> Editar nombre para mostrar
- <i class="fa-fw fa-solid fa-check"></i> Habilitar cuenta
- <i class="fa-fw fa-solid fa-ban"></i> Deshabilitar cuenta
- <i class="fa-fw fa-solid fa-arrow-up"></i> Promover a administrador
- <i class="fa-fw fa-solid fa-arrow-down"></i> Degradar a usuario regular

##### Acciones de Gestión

- <i class="fa-fw fa-solid fa-download"></i> Descargar respaldo de datos de usuario
- <i class="fa-fw fa-solid fa-key"></i> Cambiar contraseña de usuario
- <i class="fa-fw fa-solid fa-trash"></i> Eliminar cuenta

##### Usuario Nuevo

Selecciona **Usuario Nuevo** para crear una nueva cuenta de usuario.

* Nombre para Mostrar* (por ejemplo, "Juan Nieve")
* Identificador de Usuario* (solo letras minúsculas, números y guiones)
* Contraseña (opcional)
* Confirmación de Contraseña

La creación de un usuario nuevo genera automáticamente una subcarpeta en el directorio /data/ utilizando el identificador del usuario como nombre de la carpeta.

#### <i class="fa-fw fa-solid fa-right-from-bracket"></i> Cerrar Sesión

!!! Se aplica a: [modo multiusuario](/Administration/multi-user.md)
!!!

Cierra sesión de tu sesión actual.

### Búsqueda de Configuración

Una barra de búsqueda conveniente que te ayuda a encontrar rápidamente configuraciones específicas:
* Escribe cualquier palabra clave para filtrar y resaltar configuraciones en cualquier lugar de Configuración de Usuario
* Busca en nombres y descripciones de configuración
* Ayuda a navegar la configuración compleja de manera más eficiente

## Tema de Interfaz

Cambia la apariencia de la interfaz de chat según tus preferencias.

Para más información sobre la configuración en esta sección de <i class="fa-fw fa-solid fa-user-gear" title="Ícono de Configuración de Usuario"></i> **Configuración de Usuario**, consulta [Personalización de Interfaz](uicustomization.md#ui-theme).

## Manejo de Personajes

* **Subtítulo de Lista de Personajes**: Elige qué información adicional mostrar bajo los nombres de personajes en la lista [<i class="fa-fw fa-solid fa-address-card" title="Ícono de Personajes"></i> Personajes](/Usage/Characters/characterdesign.md):
    - Versión del Personaje
    - Creado por
* **Importar Etiquetas de Tarjeta**: Controla cómo se manejan las etiquetas al importar tarjetas de personaje:
    - Preguntar - Mostrar diálogo para cada importación
    - Ninguno - No importar etiquetas
    - Todo - Importar todas las etiquetas
    - Existente - Solo importar etiquetas que ya existen
* **Búsqueda Avanzada de Personajes**: Cuando está habilitada, usa coincidencia difusa y busca en todos los campos de datos del personaje, no solo en nombres.
* **Preferir Prompt de Personaje**: Si está habilitado, usa el override de Prompt del Sistema de la tarjeta de personaje cuando esté disponible.
* **Preferir Instrucciones de Personaje**: Si está habilitado, usa el override de Instrucciones Post-Historial de la tarjeta de personaje cuando esté disponible.
* **Nunca redimensionar avatares**: Evita el recorte/redimensionamiento de imágenes de personaje importadas. Cuando está deshabilitado, las imágenes se redimensionan a 512x768.
* **Mostrar nombres de archivo de avatar**: Muestra los nombres de archivo reales de los avatares de personaje en la lista de personajes.
* **Modo Libre de Spoilers**: Oculta las definiciones de personaje detrás de un botón de spoiler en el panel del editor.

## Miscelánea

* **Recargar Chat**: Recarga y redibuja el chat actual.
* **[Menú de Depuración](#debug-menu)**: Accede a opciones de depuración.
* **Smooth Streaming**: Suaviza la generación en streaming mostrando el texto letra por letra. Incluye control deslizante de velocidad.
* **Stream Fade-In**: Aplica un efecto de desvanecimiento al texto en streaming. Se puede usar con o sin Smooth Streaming.
* **[Sonido de Mensaje](uicustomization.md#message-sound)**: Reproduce un sonido cuando se completa la generación de mensajes.
    - **Solo Sonido de Fondo**: Solo reproduce sonidos cuando la pestaña del navegador no está enfocada.
* **API URLs Relajadas**: Reduce los requisitos de formato para URLs de API.
* **Diálogo de Importación de Lorebook**: Muestra diálogo de importación para World Info/Lorebook al importar personajes con tradición incrustada.
* **Auto-seleccionar Texto de Entrada**: Selecciona automáticamente el texto en ciertos campos de entrada cuando se hace clic.
* **Atajos de Markdown**: Habilita atajos de teclado para formato markdown.
* **Restaurar Entrada de Usuario**: Preserva la entrada de usuario no guardada cuando se actualiza la página.
* **MovingUI**: Permite reposicionar elementos de la interfaz arrastrándolos (solo PC).
    - <i class="fa-solid fa-recycle" title="Ícono de Reinicio"></i> Botón **Reiniciar** para restaurar posiciones predeterminadas
    - Sistema de preajustes para guardar/cargar diseños de interfaz

## Manejo de Chat/Mensajes

### Configuración de Visualización de Mensajes

Controla cómo se cargan y muestran los mensajes en la interfaz de chat. Estas configuraciones afectan la experiencia general del chat y el rendimiento.
* **# Mensajes a Cargar**: Número de mensajes del historial de chat a cargar antes de la paginación (0 = Todo)
* **Streaming FPS**: Velocidad de actualización del texto en streaming (5-100 FPS)
* **Comportamiento de Mensajes de Ejemplo**:
    - Empujar gradualmente hacia fuera
    - Siempre incluir ejemplos
    - Nunca incluir ejemplos

### Controles de Entrada y Respuesta

Configuraciones que determinan cómo se envían los mensajes y cómo continúa la IA sus respuestas.
* **Entrar para Enviar**: Elige entre Deshabilitado, Automático (PC) o Habilitado
* **"Enviar" para Continuar**: Usa el botón Enviar para continuar las respuestas de IA
* **Botón "Continuar" Rápido**: Muestra botón para extender el último mensaje de la IA
* **Botón "Suplantar" Rápido**: Muestra botón para suplantación de personaje de un único mensaje
* **Deslizamientos**: Muestra botones de flecha para respuestas alternativas de IA (PC y móvil)
* **Gestos**: Habilita gestos de deslizamiento para generación (Solo móvil)

### Auto-Gestión

Características automatizadas que ayudan a gestionar el flujo de chat y el contenido.
* **Auto-cargar Último Chat**: Carga automáticamente el chat más reciente al iniciar
* **Auto-desplazar Chat**: Desplazarse automáticamente a los mensajes más nuevos
* **Auto-guardar Ediciones de Mensaje**: Guardar ediciones de mensaje sin confirmación
* **Confirmar eliminación de mensaje**: Solicitar confirmación antes de eliminar mensajes
* **Auto-corregir Markdown**: Corregir automáticamente el formato markdown

#### Auto-swipe

Rechaza automáticamente y regenera mensajes de IA basado en criterios configurables.
* **Habilitar Auto-swipe**: Alternancia maestra para la función auto-swipe
* **Longitud mínima del mensaje generado**: Activa un auto-swipe si el mensaje es más corto que este valor
* **Palabras en lista negra**: Lista de palabras que pueden activar auto-swipe, separadas por comas
* **Recuento de palabras en lista negra para deslizar**: Número mínimo de palabras en lista negra que deben detectarse para activar un auto-swipe

#### Auto-Continue

Continúa automáticamente una respuesta si el modelo se detuvo antes de alcanzar una cierta longitud.

Esto permite que tu IA escriba una respuesta larga en múltiples partes, para que puedas tener una [configuración de longitud de respuesta](/Usage/Common-Settings.md#response-tokens) corta mientras aún obtienes respuestas largas.

No hará que la IA escriba más de lo que habría escrito de otra manera. Pedir que la IA continúe un mensaje que considera "terminado" generalmente no funciona. Consulta [¿Cómo hacer que la IA escriba más?](/Usage/faq.md#how-to-make-the-ai-write-more) para obtener otras ideas.

* **Habilitar Auto-continue**: Alternancia maestra para continuación automática
* **Permitir para APIs de Chat Completion**: Habilita la funcionalidad auto-continue para endpoints de Chat Completion API
* **Longitud objetivo (tokens)**: La longitud de mensaje deseada en tokens - activará continuar si el mensaje es más corto que este valor (0-1024)

### Formato y Visualización de Mensajes

Controla cómo se formatean los mensajes y qué contenido se muestra.
* **Prohibir Multimedia Externa**: Bloquear multimedia incrustada de dominios externos
* **Mostrar {\{char}}: en respuestas**: Retener prefijo de nombre de personaje en respuestas si se genera
* **Mostrar {\{user}}: en respuestas**: Retener prefijo de nombre de usuario en respuestas si se genera
* **Mostrar etiquetas en respuestas**: Permitir que (algunas) etiquetas HTML en respuestas se muestren como HTML
* **Relajar recorte de mensaje en Grupos**: Permitir que la IA hable por otros personajes en chats grupales, en lugar de detener la generación de respuestas
* **Mostrar cola de chat grupal**: Mostrar orden de respuesta en la lista de personajes para chats grupales
* **Fijar estilos de mensaje de saludo**: Siempre renderizar etiquetas de estilo de saludos, incluso si el mensaje está descargado debido a carga perezosa.

### Inspección de Prompt y Depuración

* **Registrar prompts en consola**: Mostrar prompts en la consola del navegador
* **Solicitar probabilidades de token**: Solicitar probabilidades de token para respuestas de IA desde la API. Donde sea disponible, estas se pueden ver en <i class="fa-solid fa-bars" title="Ícono de Menú Burger"></i> [Probabilidades de Token](../../Usage/Chatting/index.md#token-probabilities-panel).

### Autocompletar

- Ocultar detalles automáticamente
- Estilo de coincidencia (Empieza con/Incluye/Difuso)
- Estilo visual (Tema/Oscuro/Claro)
- Opciones de selección de teclado
- Escalado de fuente
- Controles de ancho

## Configuración de STscript

Opciones de configuración para el [analizador STscript](/For_Contributors/st-script.md#parser-flags).

### STRICT_ESCAPING

* Los pipes no necesitan escaparse en valores entrecomillados.
* Una barra invertida frente a un símbolo puede escaparse para proporcionar la barra invertida literal seguida del símbolo funcional.

Consulta [Escape Estricto](/For_Contributors/st-script.md#strict-escaping) para más información.

### REPLACE_GETVAR

Ayuda a evitar doble-sustituciones cuando los valores de variables contienen texto que podría interpretarse como macros.

Consulta [Reemplazar Macros de Variables](/For_Contributors/st-script.md#replace-variable-macros) para más información.

## Menú de Limpieza

El menú de Limpieza proporciona una herramienta de mantenimiento de datos que te ayuda a identificar y eliminar archivos innecesarios de tu instalación de SillyTavern. Esta función ayuda a mantener tu directorio de datos organizado y puede liberar un espacio de disco significativo.

!!! warning "Advertencia Importante"
La herramienta de limpieza eliminará permanentemente archivos. **¡Esta acción no se puede deshacer!**

Las cargas manuales en los directorios `/data/user/files/` y `/data/user/images/` se eliminarán si no están asociadas con mensajes de chat o entradas del Banco de Datos.

Si no estás seguro, haz una copia de seguridad de tus datos antes de usar el menú de Limpieza.
!!!

### Cómo Usar Limpieza

1. Haz clic en el botón **Limpieza** bajo la sección **Miscelánea**
2. Haz clic en **Escanear** para analizar tu instalación. Esto puede tomar algo de tiempo dependiendo del tamaño de tu directorio de datos
3. Revisa las categorías de archivos encontrados
4. Usa **Ver** para obtener una vista previa del contenido del archivo antes de la eliminación
5. Usa **Descargar** para guardar archivos antes de la eliminación
6. Elimina archivos individuales o categorías completas según sea necesario

### Categorías de Limpieza

La herramienta de Limpieza escanea archivos sueltos en las siguientes categorías:

#### Archivos

* **Qué encuentra**: Archivos que no están asociados con mensajes de chat o entradas del Banco de Datos
* **Ubicación**: `/data/<user-handle>/user/files/`
* **Riesgo**: ⚠️ **ELIMINARÁ CARGAS MANUALES** que no se refieran en chats
* **Cuándo limpiar**: Seguro de eliminar si no necesitas archivos no referenciados

#### Imágenes

* **Qué encuentra**: Imágenes que no están asociadas con mensajes de chat
* **Ubicación**: `/data/<user-handle>/user/images/`
* **Riesgo**: ⚠️ **ELIMINARÁ CARGAS MANUALES** que no se refieran en chats
* **Cuándo limpiar**: Seguro de eliminar si no necesitas imágenes no referenciadas

#### Chats

* **Qué encuentra**: Archivos de chat asociados con personajes eliminados
* **Ubicación**: `data/<user-handle>/chats/`
* **Riesgo**: ⚠️ **Los chats huérfanos se perderán permanentemente**
* **Cuándo limpiar**: Seguro de eliminar si has eliminado intencionalmente personajes y ya no necesitas sus historiales de chat

#### Chats Grupales

* **Qué encuentra**: Archivos de chat asociados con grupos eliminados
* **Ubicación**: `data/<user-handle>/group chats/`
* **Riesgo**: ⚠️ **Los chats grupales huérfanos se perderán permanentemente**
* **Cuándo limpiar**: Seguro de eliminar si has eliminado intencionalmente grupos y ya no necesitas sus historiales de chat

#### Miniaturas de Avatar

* **Qué encuentra**: Miniaturas para avatares de personajes faltantes o eliminados
* **Ubicación**: `data/<user-handle>/thumbnails/avatar`
* **Riesgo**: ✅ **Seguro de eliminar** - las miniaturas se regeneran automáticamente cuando sea necesario
* **Cuándo limpiar**: Siempre seguro de limpiar, ayuda a liberar espacio

#### Miniaturas de Fondo

* **Qué encuentra**: Miniaturas para fondos faltantes o eliminados
* **Ubicación**: `data/<user-handle>/thumbnails/bg`
* **Riesgo**: ✅ **Seguro de eliminar** - las miniaturas se regeneran automáticamente cuando sea necesario
* **Cuándo limpiar**: Siempre seguro de limpiar, ayuda a liberar espacio

#### Copias de Seguridad de Chat

* **Qué encuentra**: Copias de seguridad de chat generadas automáticamente
* **Ubicación**: `data/<user-handle>/backups/chat_*`
* **Riesgo**: ⚠️ **Los archivos de respaldo se perderán permanentemente**
* **Cuándo limpiar**: Considera mantener respaldos recientes, pero los más antiguos se pueden eliminar de forma segura

#### Copias de Seguridad de Configuración

* **Qué encuentra**: Copias de seguridad de configuración generadas automáticamente
* **Ubicación**: `data/<user-handle>/backups/settings_*`
* **Riesgo**: ⚠️ **Los archivos de copia de seguridad de configuración se perderán permanentemente**
* **Cuándo limpiar**: Considera mantener respaldos recientes, pero los más antiguos se pueden eliminar de forma segura

## Menú de Depuración

!!!warning Estas funciones están destinadas solo para usuarios avanzados.

No las uses a menos que entiendas completamente sus consecuencias.
!!!

El Menú de Depuración proporciona funcionalidad para propósitos de solución de problemas, mantenimiento y desarrollo. Estas funciones deben usarse con precaución ya que pueden impactar significativamente tu instalación de SillyTavern.

Debido a que las extensiones pueden agregar funciones de depuración, las opciones disponibles variarán dependiendo de las extensiones que hayas instalado.

### Funciones de Traducción e Idioma
* **Obtener traducciones faltantes**: Analiza la configuración regional actual (o todas si English está seleccionado) para traducciones faltantes y muestra los resultados en la consola del navegador
* **Aplicar idioma**: Fuerza una actualización de la configuración de idioma actual reaplicando la configuración regional seleccionada
### Gestión de Caché y Almacenamiento
* **Limpiar caché de WebSearch**: Elimina todos los resultados de búsqueda almacenados del caché local
* **Purgar todos los índices vectoriales**: Elimina completamente todos los vectores almacenados en todas las fuentes
* **Reiniciar caché de token**: Limpia los recuentos de token almacenados, forzando la retokenización completa de todos los chats
* **Eliminar prompts itemizados**: Elimina todos los prompts itemizados del almacenamiento local
### Datos y Estadísticas
* **Actualizar Archivo de Estadísticas**: Reconstruye el archivo de estadísticas usando datos de chat existentes
* **Rellenar contadores de token**: Recalcula los recuentos de token para todos los mensajes en el chat actual
    - Útil al cambiar entre modelos con diferentes tokenizers
    - Activa la recarga de chat después de completarse
    - Solo cambios visuales, no modifica el contenido del chat
### Prueba de API y Extensión
* **Cambiar URL base de Mancer**: Modifica la URL base del servidor API Mancer
* **Probar extensión WebSearch**: Realiza una búsqueda de prueba usando la configuración actual
* **Enviar solicitud de generación**: Prueba la generación de texto usando la API actualmente seleccionada
### Herramientas de Sistema y Depuración
* **Forzar inicio de sesión**: Reinicia el proceso de bienvenida
* **Alternar rastreo de eventos**: Habilita/deshabilita el rastreo de eventos para depuración
* **Copiar configuración de ST**: [En Progreso] Copia datos de configuración del sistema al portapapeles para reportes de errores

Cada función se puede ejecutar usando el botón "Ejecutar" debajo de su descripción. Considera hacer una copia de seguridad de tus datos antes de usar estas herramientas, ya que algunas operaciones no se pueden deshacer.
