---
icon: rss
order: -30
route: /usage/remoteconnections/
---

# Conexiones remotas

Con mayor frecuencia, esto es para personas que quieren usar SillyTavern en sus teléfonos móviles mientras su PC ejecuta el servidor ST dentro de la misma red WiFi.

También es el primer paso para permitir conexiones remotas desde fuera de la red local.

!!!warning
No debes usar reenvío de puertos para exponer tu servidor ST a internet. En su lugar, usa una VPN o un servicio de túnel como Cloudflare Zero Trust, ngrok o Tailscale. Consulta la guía de [VPN y Túneles](tunneling.md) para más información.
!!!

!!!danger Descargo de responsabilidad
**NUNCA ALOJES NINGUNA INSTANCIA EN INTERNET ABIERTO SIN ASEGURAR PRIMERO LAS MEDIDAS DE SEGURIDAD ADECUADAS.**

**NO SOMOS RESPONSABLES DE NINGÚN DAÑO O PÉRDIDA EN CASOS DE ACCESO NO AUTORIZADO DEBIDO A UNA IMPLEMENTACIÓN DE SEGURIDAD INADECUADA O INSUFICIENTE.**
!!!

## Permitir conexiones remotas

Por defecto, el servidor ST solo acepta conexiones desde la máquina en la que se está ejecutando (localhost). Para permitir que escuche conexiones desde otros dispositivos, establece la opción `listen` en `config.yaml` a `true`.

!!! Si buscas `config.yaml` directamente en la carpeta de SillyTavern, puedes encontrar dos archivos.
Todas las modificaciones a `config.yaml` en este documento se refieren al que está en el directorio raíz de SillyTavern (/SillyTavern/config.yaml), no a `/SillyTavern/default/config.yaml`.
!!!

```yaml
# Listen for incoming connections
listen: true
```

Cuando ST está escuchando conexiones remotas, deberías ver este mensaje en la consola:

```txt
SillyTavern is listening on IPv4: 0.0.0.0:8000
```

y alguna explicación sobre lo que eso significa.

Cuando ST **no** está escuchando conexiones remotas, deberías ver este mensaje en la consola:

```txt
SillyTavern is listening on IPv4: 127.0.0.1:8000
```

## Configuración de control de acceso

Después de habilitar la escucha de conexiones remotas, debes configurar al menos un método de control de acceso. De lo contrario, el servidor no se iniciará.

### Control de acceso basado en lista blanca

Para habilitar el control de acceso a través de una lista blanca, edita el archivo `config.yaml` en el directorio raíz de SillyTavern (`/SillyTavern/config.yaml`):

1. Inicia SillyTavern al menos una vez para generar los archivos de configuración necesarios.
2. Abre `/SillyTavern/config.yaml` en un editor de texto.
3. Encuentra la sección `whitelist` y agrega las direcciones IP que deseas permitir:
    * Lista cada dirección IP por separado.
    * Asegúrate de que `127.0.0.1` esté incluida, o no podrás conectarte desde la máquina anfitriona.
    * Soporta IPs individuales, máscaras CIDR (por ejemplo, `10.0.0.0/24`) y rangos con comodín (`*`).
4. Guarda el archivo `config.yaml`.
5. **Reinicia tu servidor SillyTavern.**

#### Ejemplo de configuración de lista blanca en `config.yaml`

1. Permitir cualquier dispositivo en la red local:

    ```yaml
    whitelist:
      - ::1
      - 127.0.0.1
      - 10.0.0.0/8
      - 172.16.0.0/12
      - 192.168.0.0/16
    ```

    Si no estás seguro del rango de direcciones de tu red local, usa la lista blanca anterior.

2. Permitir que dos dispositivos específicos se conecten:

    ```yaml
    whitelist:
      - ::1
      - 127.0.0.1
      - 192.168.0.2
      - 192.168.0.5
    ```

3. Permitir que cualquier dispositivo en la subred `192.168.0.*` se conecte:

    ```yaml
    whitelist:
      - ::1
      - 127.0.0.1
      - 192.168.0.*
    ```

4. Permitir conexiones de red para todos los dispositivos IPv4:

    ```yaml
    whitelist:
      - 0.0.0.0/0
    ```

### Deshabilitar el control de acceso basado en lista blanca

Para deshabilitar el control de acceso a través de una lista blanca:

* Establece `whitelistMode` a `false` en `/SillyTavern/config.yaml`.
* Elimina o renombra `whitelist.txt` (si existe) en la carpeta de instalación base de SillyTavern.
* Reinicia tu servidor SillyTavern.

### No recomendado: usar `whitelist.txt`

!!!info
Si `whitelist.txt` existe, tiene precedencia sobre la configuración de lista blanca en `config.yaml`.

Sin embargo, dado que todas las demás configuraciones se gestionan dentro de `config.yaml`, y `whitelist.txt` puede encontrar problemas de permisos o bloquearse, el sistema podría revertir silenciosamente a usar la lista blanca de `config.yaml`.

**Editar config.yaml directamente es tanto más simple como más confiable.**
!!!

Si aún prefieres usar whitelist.txt:

1. Crea un nuevo archivo de texto llamado `whitelist.txt` en la carpeta de instalación base de SillyTavern.
2. Ábrelo en un editor de texto y agrega las direcciones IP permitidas.
3. Guarda el archivo y reinicia tu servidor SillyTavern.

#### Ejemplo de configuración de `whitelist.txt`

```txt
10.0.0.0/8
172.16.0.0/12
192.168.0.0/16
127.0.0.1
::1
```

Esto permite que cualquier dispositivo en la red local se conecte.

### Control de acceso mediante autenticación HTTP básica

!!!warning
La autenticación HTTP básica no proporciona seguridad fuerte.

No hay limitación de tasa para prevenir ataques de fuerza bruta. Si esto es una preocupación, se recomienda usar un proxy inverso con TLS y limitación de tasa, y un [servicio de autenticación](sso.md) dedicado.
!!!

El servidor solicitará nombre de usuario y contraseña cada vez que un cliente se conecte a través de HTTP. **Esto solo funciona si las conexiones remotas (listen: true) están habilitadas.**

Para habilitar HTTP BA, abre `config.yaml` en el directorio base de SillyTavern y busca `basicAuthMode`. Establece basicAuthMode a true y configura el nombre de usuario y contraseña. Nota: `config.yaml` solo existirá si ST se ha ejecutado antes al menos una vez.

```yaml
basicAuthMode: true
basicAuthUser:
  username: "MyUsername"
  password: "MyPassword"
```

Alternativamente, puedes habilitar la autenticación básica de la siguiente manera:

```yaml
basicAuthMode: true
enableUserAccounts: true
perUserBasicAuth: true
```

En este modo `perUserBasicAuth`, el nombre de usuario y la contraseña de la autenticación básica serán los mismos que cualquier cuenta multiusuario válida que tenga una contraseña. Además, SillyTavern iniciará sesión directamente en esa cuenta. **Asegúrate de tener una cuenta con contraseña antes de habilitar `perUserBasicAuth`.**

Guarda el archivo y reinicia SillyTavern si ya se estaba ejecutando. Se te solicitará nombre de usuario y contraseña al conectarte a tu ST. Tanto el nombre de usuario como la contraseña se transmiten en texto plano. Si te preocupa esto, puedes servir ST a través de HTTPS.

### Lista blanca de hosts

Al alojar un servidor en la red sin HTTPS, es muy recomendable habilitar la verificación de host de solicitud. Esto ayuda a prevenir varios ataques, como el reenlace DNS. Por defecto, el servidor SillyTavern registrará un mensaje de consola en la primera conexión desde un host no reconocido.

### Alternar lista blanca de hosts

Para habilitar la lista blanca de hosts, edita el archivo `config.yaml` en el directorio raíz de SillyTavern:

```yaml
hostWhitelist:
    enabled: true
```

### Agregar hosts de confianza

Para agregar un nombre de host a una lista de hosts de confianza, inclúyelo en la sección `hostWhitelist.hosts`:

!!!tip Consejos
No agregues `localhost` o IPs (como `127.0.0.1` o `::1`). Estos siempre se consideran de confianza.

Para agregar un rango de hosts, usa un punto inicial. Por ejemplo, agregar `.trycloudflare.com` confiará en `trycloudflare.com` así como en cualquier subdominio como `example.trycloudflare.com`.
!!!

```yaml
hostWhitelist:
  hosts:
    - "example.com"
    - ".trycloudflare.com"
```

### Alternar mensajes de consola

Para deshabilitar mensajes de consola para hosts no reconocidos, establece la opción `hostWhitelist.scan` a `false`:

```yaml
hostWhitelist:
    scan: false
```

## Conectarse a tu instancia de SillyTavern

### Obtener la dirección IP de la máquina anfitriona de ST

Después de que la lista blanca haya sido configurada, necesitarás la IP del dispositivo que aloja ST.

Si el dispositivo que aloja ST está en la misma red wifi, usarás la IP wifi interna del host ST:

* Para Windows: botón de windows > escribe `cmd.exe` en la barra de búsqueda > escribe `ipconfig` en la consola, presiona Enter > busca el listado de `IPv4`.

Si tú (o alguien más) quiere conectarse a tu ST alojado sin estar en la misma red, necesitarás la IP pública de tu dispositivo que aloja ST.

* Mientras usas el dispositivo que aloja ST, accede a [esta página](https://whatismyipaddress.com/) y busca `IPv4`. Esto es lo que usarías para conectarte desde el dispositivo remoto.

### Conectarse al servidor ST

Cualquiera que sea la IP que hayas obtenido para tu situación, pondrás esa dirección IP y número de puerto en el navegador web del dispositivo remoto.

Una dirección típica para un host ST en la misma red wifi se vería como:

`http://192.168.0.5:8000`

Usa http:// NO https://

### Registro de conexiones

Las nuevas conexiones al servidor se muestran en la ventana de consola y se registran en el archivo `access.log` en el directorio de datos de SillyTavern.

Un mensaje de consola para un navegador en la misma máquina que el servidor se ve como:

```txt
New connection from 127.0.0.1; User Agent: ...
```

Un mensaje de consola para un navegador en una máquina diferente en la misma red que el servidor podría verse como:

```txt
New connection from 192.168.116.187; User Agent: ...
```

Si una conexión es rechazada, el mensaje de consola se verá como:

```txt
New connection from 192.168.116.211; User Agent: ...

Forbidden: Connection attempt from 192.168.116.211. If you are attempting to connect,
please add your IP address in whitelist or disable whitelist mode in config.yaml in
root of SillyTavern folder.
```

`access.log` contendrá la información de conexión, con marcas de tiempo, pero no si la conexión fue aceptada o rechazada.

### Solución de problemas

¿Aún no puedes conectarte?

* Si el intento de conexión [aparece en la consola](#connection-logging), pero está prohibido, es un [problema de lista blanca](#whitelist-based-access-control).
* Si ST está escuchando conexiones remotas pero el intento de conexión no aparece en la consola, es un [problema de red](#network-issues).
* Si ST no está escuchando conexiones remotas, es un [problema de lectura](#allowing-remote-connections).

#### Problemas de red

* En Windows, la aplicación puede estar bloqueada por el firewall de la aplicación. La forma más rápida de solucionar esto es desinstalar y reinstalar node.js, y cuando el firewall lo solicite, permitirle acceso a la red. De lo contrario, deberás permitir manualmente la aplicación node.js a través del firewall de aplicaciones de Windows.
* En Windows 11, habilita el tipo de perfil de Red Privada en Configuración > Red e Internet > Ethernet. Esto es MUY importante para Windows 11, de lo contrario, no podrás conectarte incluso con las reglas de firewall mencionadas anteriormente.
* En Linux, es posible que necesites permitir el puerto a través del firewall. El comando para hacer esto es `sudo ufw allow 8000`. Esto permitirá tráfico en el puerto 8000.

No modifiques la configuración de reenvío de puertos en tu enrutador. Esto no es necesario para acceder a ST dentro de tu red local, y puede exponer tu servidor a internet.

Si estás intentando acceder a tu servidor ST desde [fuera de tu red local](remote-connections.md), y no funciona, identifica si el problema está entre el dispositivo remoto y el punto final del túnel/VPN, o entre el punto final del túnel en el servidor y el servicio ST. De lo contrario, pasarás mucho tiempo solucionando el problema equivocado.

## HTTPS

### Iniciar SillyTavern con TLS/SSL

!!!tip
SSL también se puede configurar usando el archivo `config.yaml`: [Configuración SSL](/Administration/config-yaml.md#ssl-configuration).
!!!

Para cifrar el tráfico desde y hacia tu instancia de ST, inicia el servidor con la bandera `--ssl`.

Ejemplo:

```bash
node server.js --ssl
```

Por defecto, ST buscará tus certificados dentro de la carpeta `certs`. Si tus archivos están ubicados en otro lugar, puedes usar los argumentos `--keyPath` y `--certPath`.

Ejemplo:

```bash
node server.js --ssl --keyPath /home/user/certificates/privkey.pem --certPath /home/user/certificates/cert.pem
```

El usuario con el que estás ejecutando SillyTavern requiere permisos de lectura en los archivos de certificado.

### Cómo obtener un certificado

La forma más simple y rápida de obtener un certificado es usando [certbot](https://letsencrypt.org/getting-started/).
