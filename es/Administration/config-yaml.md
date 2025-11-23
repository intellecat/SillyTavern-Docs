---
icon: cpu
route: /administration/config-yaml/
---

# Archivo de Configuración

!!!warning Aviso

Esta documentación puede estar obsoleta, incompleta o ser incorrecta. Por favor, consulte el [config.yaml predeterminado](https://github.com/SillyTavern/SillyTavern/blob/release/default/config.yaml) en su instalación para obtener la lista más actualizada de configuraciones.

**ADVERTENCIA: NO EDITE LA CONFIGURACIÓN PREDETERMINADA DIRECTAMENTE. ESTO NO TENDRÁ NINGÚN EFECTO POSITIVO. EDITE SU COPIA EN LA RAÍZ DEL REPOSITORIO EN SU LUGAR.**
!!!

`config.yaml` es el archivo de configuración principal para el servidor de SillyTavern que puede encontrar en el directorio raíz del repositorio después de [completar la instalación](/Installation/index.md). Es un archivo YAML que contiene varias configuraciones, como opciones de red, seguridad y específicas del backend. **Los cambios realizados en este archivo tendrán efecto después de reiniciar el servidor.**

Las nuevas configuraciones que se agregan upstream se rellenan automáticamente con valores predeterminados cuando ejecuta `npm install` (específicamente, el script `post-install.js`) después de [actualizar el repositorio](/Installation/Updating/index.md). Luego puede modificar estas configuraciones según sea necesario.

Para configuraciones anidadas, se utiliza la notación de puntos para indicar la jerarquía. Por ejemplo, `protocol.ipv6: false` se refiere a la configuración `ipv6` bajo la sección `protocol` con un valor de `false`.

```yaml
protocol:
  ipv6: false
```

## Argumentos de Línea de Comandos

Puede pasar argumentos de línea de comandos al iniciar el servidor de SillyTavern para anular algunas configuraciones en [config.yaml](../Administration/config-yaml.md).

### Ejemplos

```shell
node server.js --port 8000 --listen false
# o
npm run start -- --port 8000 --listen false
# o (solo Windows)
Start.bat --port 8000 --listen false
```

### Argumentos soportados

!!!tip
Ninguno de los argumentos es obligatorio. Si no los proporciona, SillyTavern utilizará las configuraciones en `config.yaml`.
!!!

| Opción                          | Descripción                                                                          | Tipo     |
|---------------------------------|--------------------------------------------------------------------------------------|----------|
| `--version`                     | Muestra el número de versión                                                         | boolean  |
| `--global`                      | Fuerza el uso de rutas a nivel del sistema para datos de la aplicación              | boolean  |
| `--configPath`                  | Anula la ruta al archivo config.yaml (solo modo independiente)                      | string   |
| `--dataRoot`                    | Establece el directorio raíz para el almacenamiento de datos (solo modo independiente) | string   |
| `--port`                        | Establece el puerto bajo el cual se ejecutará SillyTavern                           | number   |
| `--listen`                      | Hace que SillyTavern escuche en todas las interfaces de red                         | boolean  |
| `--whitelist`                   | Habilita el modo de lista blanca                                                     | boolean  |
| `--basicAuthMode`               | Habilita la autenticación básica                                                     | boolean  |
| `--enableIPv4`                  | Habilita el protocolo IPv4                                                           | boolean  |
| `--enableIPv6`                  | Habilita el protocolo IPv6                                                           | boolean  |
| `--listenAddressIPv4`           | Especifica la dirección IPv4 en la que escuchar                                      | string   |
| `--listenAddressIPv6`           | Especifica la dirección IPv6 en la que escuchar                                      | string   |
| `--dnsPreferIPv6`               | Prefiere IPv6 para DNS                                                               | boolean  |
| `--ssl`                         | Habilita SSL                                                                         | boolean  |
| `--certPath`                    | Establece la ruta a su archivo de certificado                                        | string   |
| `--keyPath`                     | Establece la ruta a su archivo de clave privada                                      | string   |
| `--browserLaunchEnabled`        | Lanza automáticamente SillyTavern en el navegador                                    | boolean  |
| `--browserLaunchHostname`       | Establece el nombre de host para el lanzamiento del navegador                        | string   |
| `--browserLaunchPort`           | Anula el puerto para el lanzamiento del navegador                                    | string   |
| `--browserLaunchAvoidLocalhost` | Evita usar 'localhost' para el lanzamiento del navegador en modo automático          | boolean  |
| `--corsProxy`                   | Habilita el proxy CORS                                                               | boolean  |
| `--requestProxyEnabled`         | Habilita el uso de un proxy para solicitudes salientes                              | boolean  |
| `--requestProxyUrl`             | Establece la URL del proxy de solicitudes (protocolos HTTP o SOCKS)                 | string   |
| `--requestProxyBypass`          | Establece la lista de omisión del proxy de solicitudes (lista separada por espacios de hosts) | array    |
| `--disableCsrf`                 | Deshabilita la protección CSRF (NO RECOMENDADO)                                     | boolean  |

## Variables de Entorno

La configuración también se puede establecer a través de variables de entorno, que anularán los valores en el archivo `config.yaml`.

Las variables de entorno deben tener el prefijo `SILLYTAVERN_` y usar letras mayúsculas para los nombres de las configuraciones. Por ejemplo, la configuración `dataRoot` se puede anular con la variable de entorno `SILLYTAVERN_DATAROOT`.

Las configuraciones anidadas deben separarse con guiones bajos. Por ejemplo, `protocol.ipv6` se puede anular con la variable de entorno `SILLYTAVERN_PROTOCOL_IPV6`.

!!!warning
Las configuraciones que esperan arrays u objetos deben convertirse a cadenas JSON. Por ejemplo, para anular la configuración `whitelist` con la variable de entorno `SILLYTAVERN_WHITELIST`, debe establecerla como una cadena JSON: `SILLYTAVERN_WHITELIST='["127.0.0.1", "::1"]'`.
!!!

Si está utilizando Node.js v20 o posterior, también puede almacenar variables de entorno en un archivo `.env` y pasarlo al servidor con la bandera `--env-file`. Por ejemplo, para usar el archivo `.env` ubicado en la raíz del repositorio, puede iniciar el servidor con el siguiente comando:

```bash
node --env-file=.env server.js
```

Alternativamente, pase las variables de entorno directamente a través de la línea de comandos:

```bash
SILLYTAVERN_LISTEN=true SILLYTAVERN_PORT=8000 node server.js
```

Consulte más información sobre el uso de variables de entorno en la [documentación de Node.js](https://nodejs.org/en/learn/command-line/how-to-read-environment-variables-from-nodejs).

## Configuración de Datos

| Configuración | Descripción | Predeterminado | Valores Permitidos |
|---------|-------------|---------|-----------------|
| `dataRoot` | Directorio raíz para el almacenamiento de datos del usuario (solo modo independiente) | `./data` | Cualquier ruta de directorio válida |
| `skipContentCheck` | Omitir comprobaciones de nuevo contenido predeterminado | `false` | `true`, `false` |
| `enableDownloadableTokenizers` | Habilitar descargas de tokenizadores bajo demanda | `true` | `true`, `false` |

## Configuración de Registro

| Configuración | Descripción | Predeterminado | Valores Permitidos |
|---------|-------------|---------|------------------|
| `logging.minLogLevel` | Nivel mínimo de registro para mostrar en el terminal | `0` (DEBUG) | (DEBUG = 0, INFO = 1, WARN = 2, ERROR = 3) |
| `logging.enableAccessLog` | Escribir registros de acceso del servidor en un archivo y la consola | `true` | `true`, `false` |

## [Configuración de Red](/Administration/remote-connections.md)

| Configuración | Descripción | Predeterminado | Valores Permitidos |
|---------|-------------|---------|-----------------|
| `listen` | Habilitar la escucha de conexiones entrantes | `false` | `true`, `false` |
| `port` | Puerto de escucha del servidor | `8000` | Cualquier número de puerto válido (1-65535) |
| `protocol.ipv4` | Habilitar la escucha en el protocolo IPv4 | `true` | `true`, `false`, `auto` |
| `protocol.ipv6` | Habilitar la escucha en el protocolo IPv6 | `false` | `true`, `false`, `auto` |
| `listenAddress.ipv4` | Escuchar en una dirección IPv4 específica | `0.0.0.0` | Dirección IPv4 válida |
| `listenAddress.ipv6` | Escuchar en una dirección IPv6 específica | `'[::]'` | Dirección IPv6 válida |
| `dnsPreferIPv6` | Preferir IPv6 para la resolución DNS | `false` | `true`, `false` |

## Configuración SSL

| Configuración | Descripción | Predeterminado | Valores Permitidos |
|---------|-------------|---------|------------------|
| `ssl.enabled` | Habilitar cifrado SSL/TLS y protocolo HTTPS | `false` | `true`, `false` |
| `ssl.keyPath` | Ruta a la clave privada SSL (relativa al directorio del servidor) | `"./certs/privkey.pem"` | Ruta de archivo válida |
| `ssl.certPath` | Ruta al certificado SSL (relativa al directorio del servidor) | `"./certs/cert.pem"` | Ruta de archivo válida |
| `ssl.keyPassphrase` | Frase de contraseña para la clave privada SSL. Deje vacío si no es necesario | `""` | Cualquier cadena |

## Configuración de Seguridad

### Lista Blanca de IP

| Configuración | Descripción | Predeterminado | Valores Permitidos |
|---------|-------------|---------|-----------------|
| `whitelistMode` | Habilitar filtrado de lista blanca de IP | `true` | `true`, `false` |
| `enableForwardedWhitelist` | Verificar encabezados reenviados para IPs en lista blanca | `true` | `true`, `false` |
| `whitelist` | Lista de direcciones IP permitidas | `["::1", "127.0.0.1"]` | Array de direcciones IP válidas |
| `whitelistDockerHosts` | Agregar automáticamente IPs de host Docker a la lista blanca | `true` | `true`, `false` |

### Lista Blanca de Host

| Configuración | Descripción | Predeterminado | Valores Permitidos |
|---------|-------------|---------|-----------------|
| `hostWhitelist.enabled` | Habilitar lista blanca de host | `false` | `true`, `false` |
| `hostWhitelist.scan` | Registrar solicitudes entrantes de hosts no confiables | `true` | `true`, `false` |
| `hostWhitelist.hosts` | Lista de nombres de host confiables | `[]` | Array de nombres de host válidos |

### Anulaciones de Seguridad

!!!danger
**DESHABILITAR MEDIDAS DE SEGURIDAD ES ALTAMENTE DESACONSEJADO. POR FAVOR, ASEGÚRESE DE COMPRENDER LO QUE ESTÁ HACIENDO ANTES DE REALIZAR CAMBIOS.**
!!!

| Configuración | Descripción | Predeterminado | Valores Permitidos |
|---------|-------------|---------|------------------|
| `allowKeysExposure` | Permitir la exposición sin máscara de claves API en la interfaz de usuario | `false` | `true`, `false` |
| `disableCsrfProtection` | Deshabilitar la protección CSRF (no recomendado) | `false` | `true`, `false` |
| `securityOverride` | Deshabilitar comprobaciones de seguridad al inicio (no recomendado) | `false` | `true`, `false` |

## [Autenticación de Usuario](/Administration/multi-user.md)

| Configuración | Descripción | Predeterminado | Valores Permitidos |
|---------|-------------|---------|-----------------|
| `basicAuthMode` | Habilitar autenticación básica | `false` | `true`, `false` |
| `basicAuthUser.username` | Nombre de usuario de autenticación básica | `"user"` | Cualquier cadena |
| `basicAuthUser.password` | Contraseña de autenticación básica | `"password"` | Cualquier cadena |
| `enableUserAccounts` | Habilitar modo multiusuario | `false` | `true`, `false` |
| `enableDiscreetLogin` | Ocultar la lista de usuarios en la pantalla de inicio de sesión | `false` | `true`, `false` |
| `sessionTimeout` | Tiempo de espera de sesión de usuario en segundos | `-1` (deshabilitado) | Cualquier número (-1 para deshabilitar, 0 para cierre del navegador, >0 para tiempo de espera) |
| `perUserBasicAuth` | Usar credenciales de cuenta para autenticación básica | `false` | `true`, `false` |

### Inicio de Sesión Automático SSO

| Configuración | Descripción | Predeterminado | Valores Permitidos |
|---------|-------------|---------|------------------|
| `sso.autheliaAuth` | Habilitar inicio de sesión automático basado en Authelia. Ver: [SSO](/Administration/sso.md) | `false` | `true`, `false` |
| `sso.authentikAuth` | Habilitar inicio de sesión automático basado en Authentik. Ver: [SSO](/Administration/sso.md) | `false` | `true`, `false` |

## Configuración de Limitación de Tasa

| Configuración | Descripción | Predeterminado | Valores Permitidos |
|---------|-------------|---------|------------------|
| `rateLimiting.preferRealIpHeader` | Usar el encabezado X-Real-IP en lugar de la IP del socket para la limitación de tasa | `false` | `true`, `false` |

## Configuración de Proxy de Solicitudes

| Configuración | Descripción | Predeterminado | Valores Permitidos |
|---------|-------------|---------|-----------------|
| `requestProxy.enabled` | Habilitar proxy para solicitudes salientes | `false` | `true`, `false` |
| `requestProxy.url` | URL del servidor proxy | `null` | URL de proxy válida (p. ej., `"socks5://username:password@example.com:1080"`) |
| `requestProxy.bypass` | Hosts para omitir el proxy | `["localhost", "127.0.0.1"]` | Array de nombres de host/IPs |

## Configuración de Proxy CORS

!!!
Un proxy CORS habilitado puede ser requerido por algunas extensiones. No es requerido por ninguna característica incorporada.
!!!

| Configuración | Descripción | Predeterminado | Valores Permitidos |
|---------|-------------|---------|-----------------|
| `enableCorsProxy` | Habilitar middleware de proxy CORS | `false` | `true`, `false` |

## Configuración de Lanzamiento del Navegador

> Anteriormente conocido como configuraciones de "Autorun".

| Configuración | Descripción | Predeterminado | Valores Permitidos |
|---------|-------------|---------|-----------------|
| `browserLaunch.enabled` | Abrir el navegador automáticamente al iniciar el servidor | `true` | `true`, `false` |
| `browserLaunch.browser` | Navegador a usar para abrir la URL | `"default"` | `"default"`, `"chrome"`, `"firefox"`, `"edge"`, `"brave"` |
| `browserLaunch.hostname` | Anular el nombre de host para el lanzamiento del navegador | `"auto"` | `"auto"`, cualquier nombre de host válido (p. ej., `"localhost"`, `"st.example.com"`) |
| `browserLaunch.port` | Anular el puerto para el lanzamiento del navegador | `-1` | `-1` (usar puerto del servidor), cualquier número de puerto válido |
| `browserLaunch.avoidLocalhost` | Evitar usar 'localhost' en la URL de lanzamiento | `false` | `true`, `false` |

## Configuración de Rendimiento

| Configuración | Descripción | Predeterminado | Valores Permitidos |
|---------|-------------|---------|------------------|
| `performance.lazyLoadCharacters` | Carga diferida de datos de personajes | `true` | `true`, `false` |
| `performance.useDiskCache` | Habilitar almacenamiento en caché de disco para tarjetas de personajes | `true` | `true`, `false` |
| `performance.memoryCacheCapacity` | Capacidad máxima de caché de memoria | `100mb` | Tamaño legible por humanos (p. ej., `100mb`, `1gb`) |

## Configuración de Cache Buster

!!!warning
¡Requiere localhost o un dominio con HTTPS, de lo contrario no funcionará!
!!!

| Configuración | Descripción | Predeterminado | Valores Permitidos |
|---------|-------------|---------|------------------|
| `cacheBuster.enabled` | Limpiar caché del navegador en la primera carga o después de cargar archivos de imagen | `false` | `true`, `false` |
| `cacheBuster.userAgentPattern` | Solo limpiar la caché para agentes de usuario que coincidan con el patrón regex especificado. Ejemplo: `'firefox'` (sin distinción de mayúsculas y minúsculas). | `''` | Cualquier cadena regex válida |

## Configuración de Miniaturas

| Configuración | Descripción | Predeterminado | Valores Permitidos |
|---------|-------------|---------|-----------------|
| `thumbnails.enabled` | Habilitar generación de miniaturas | `true` | `true`, `false` |
| `thumbnails.quality` | Calidad de miniaturas JPEG | `95` | 0-100 |
| `thumbnails.format` | Formato de imagen para miniaturas | `jpg` | `jpg`, `png` |
| `thumbnails.dimensions.bg` | Tamaño de miniaturas de fondo | `[160, 90]` | Array de dos números (ancho, alto) |
| `thumbnails.dimensions.avatar` | Tamaño de miniaturas de avatar | `[96, 144]` |  Array de dos números (ancho, alto) |
| `thumbnails.dimensions.persona` | Tamaño de miniaturas de persona | `[96, 144]` |  Array de dos números (ancho, alto) |

## Configuración de Copias de Seguridad

| Configuración | Descripción | Predeterminado | Valores Permitidos |
|---------|-------------|---------|-----------------|
| `backups.chat.enabled` | Habilitar copias de seguridad automáticas de chat | `true` | `true`, `false` |
| `backups.chat.checkIntegrity` | Verificar la integridad de los archivos de chat antes de guardar | `true` | `true`, `false` |
| `backups.common.numberOfBackups` | Número de copias de seguridad a mantener | `50` | Cualquier entero positivo |
| `backups.chat.throttleInterval` | Intervalo de limitación de copia de seguridad (ms) | `10000` | Cualquier entero positivo |
| `backups.chat.maxTotalBackups` | Máximo total de copias de seguridad de chat a mantener | `-1` | Cualquier entero positivo o -1 |

## [Configuración de Extensiones](/extensions/index.md)

| Configuración | Descripción | Predeterminado | Valores Permitidos |
|---------|-------------|---------|-----------------|
| `extensions.enabled` | Habilitar extensiones de interfaz de usuario | `true` | `true`, `false` |
| `extensions.autoUpdate` | Actualizar automáticamente extensiones (si está habilitado por el manifiesto de la extensión) | `true` | `true`, `false` |
| `extensions.models.autoDownload` | Habilitar descargas automáticas de modelos | `true` | `true`, `false` |
| `extensions.models.classification` | ID de modelo de HuggingFace para clasificación | `"Cohee/distilbert-base-uncased-go-emotions-onnx"` | ID de modelo válido |
| `extensions.models.captioning` | ID de modelo de HuggingFace para subtitulado de imágenes | `"Xenova/vit-gpt2-image-captioning"` | ID de modelo válido |
| `extensions.models.embedding` | ID de modelo de HuggingFace para embeddings | `"Cohee/jina-embeddings-v2-base-en"` | ID de modelo válido |
| `extensions.models.speechToText` | ID de modelo de HuggingFace para voz a texto | `"Xenova/whisper-small"` | ID de modelo válido |
| `extensions.models.textToSpeech` | ID de modelo de HuggingFace para texto a voz | `"Xenova/speecht5_tts"` | ID de modelo válido |

## [Plugins del Servidor](/For_Contributors/Server-Plugins.md)

| Configuración | Descripción | Predeterminado | Valores Permitidos |
|---------|-------------|---------|-----------------|
| `enableServerPlugins` | Habilitar plugins del lado del servidor | `false` | `true`, `false` |
| `enableServerPluginsAutoUpdate` | Intentar actualizar automáticamente los plugins del servidor al inicio | `true` | `true`, `false` |

## [Configuración de Integración de API](/Usage/API_Connections/index.md)

### Configuración de OpenAI

| Configuración | Descripción | Predeterminado | Valores Permitidos |
|---------|-------------|---------|-----------------|
| `promptPlaceholder` | Mensaje predeterminado para prompts vacíos | `"[Start a new chat]"` | Cualquier cadena |
| `openai.randomizeUserId` | Aleatorizar el ID de usuario para llamadas a la API | `false` | `true`, `false` |
| `openai.captionSystemPrompt` | Mensaje del sistema para completado de subtítulos | `""` | Cualquier cadena |

### Configuración de MistralAI

| Configuración | Descripción | Predeterminado | Valores Permitidos |
|---------|-------------|---------|-----------------|
| `mistral.enablePrefix` | Habilitar prellenado de respuestas. **El prefijo se repetirá en la respuesta** | `false` | `true`, `false` |

### Configuración de Ollama

| Configuración | Descripción | Predeterminado | Valores Permitidos |
|---------|-------------|---------|-----------------|
| `ollama.keepAlive` | Duración de mantenimiento del modelo activo (segundos) | `-1` | `-1` (indefinido), `0` (descarga inmediata), entero positivo |
| `ollama.batchSize` | Controla el parámetro "num_batch" (tamaño de lote) de la solicitud de generación | `-1` | `-1` (predeterminado del modelo), entero positivo |

### Configuración de Claude

!!!warning **¡IMPORTANTE!**

Úselo con precaución y solo cuando el prefijo del prompt sea estático y no cambie entre solicitudes. El macro \{\{random\}\}, lorebooks, vectores, resúmenes, etc. probablemente invalidarán la caché y solo desperdiciará dinero en fallos de caché. El comportamiento puede ser impredecible y no se pueden ni se harán garantías.

Ver: [Prompt Caching](https://docs.anthropic.com/en/docs/build-with-claude/prompt-caching)
!!!

| Configuración | Descripción | Predeterminado | Valores Permitidos |
|---------|-------------|---------|-----------------|
| `claude.enableSystemPromptCache` | Habilitar almacenamiento en caché de prompt del sistema | `false` | `true`, `false` |
| `claude.cachingAtDepth` | Habilitar almacenamiento en caché del historial de mensajes | `-1` | `-1` (deshabilitado), `0` o entero positivo |
| `claude.extendedTTL` | Usar TTL de 1h en lugar del predeterminado de 5m. Tenga en cuenta que esto también aumenta el costo de la solicitud. | `false` | `true`, `false` |

### Configuración de Google AI Studio

| Configuración | Descripción | Predeterminado | Valores Permitidos |
|---------|-------------|---------|-----------------|
| `gemini.apiVersion` | Versión del endpoint de API | `v1beta` | `v1beta`, `v1alpha` |

### Configuración de DeepL

| Configuración | Descripción | Predeterminado | Valores Permitidos |
|---------|-------------|---------|-----------------|
| `deepl.formality` | Nivel de formalidad de la traducción | `"default"` | `"default"`, `"more"`, `"less"`, `"prefer_more"`, `"prefer_less"` |
