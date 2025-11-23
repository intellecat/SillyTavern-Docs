---
label: VPNs y Túneles
order: -40
icon: lock
route: /administration/tunneling/
---

VPNs y túneles son una forma segura de acceder a tu red doméstica desde cualquier lugar del mundo. Esta guía te mostrará cómo usar un VPN o un túnel para acceder a tu instancia de SillyTavern desde cualquier lugar.

## Métodos

1. Usa un **VPN casero**.

   Varios routers vienen con la capacidad de alojar un servidor VPN (principalmente OpenVPN o WireGuard) en la página de administración del router. Consulta el manual de tu router para configurar un VPN y agregar tus dispositivos al VPN. Una vez conectado, simplemente ve a la IP privada que has configurado para SillyTavern y podrás conectarte sin problemas. Más fácil para usuarios y para uso en Windows.

2. Usa [**Cloudflare Zero Trust**](https://developers.cloudflare.com/cloudflare-one/).

   Cloudflare Zero Trust es una función organizacional gratuita en Cloudflare que te permite agregar 50 usuarios. Esto enviará tu tráfico a través de Cloudflare y al agregar tu PC con ST como un túnel usando `cloudflared`, puedes conectarte a tu instancia de ST como si estuvieras en casa.

   Ten en cuenta que después de crear un túnel, tendrás que agregar una ruta a las direcciones IP privadas de tu router y calcular valores IP CIDR para tener acceso local completo sobre la marcha usando Cloudflare Zero Trust.

3. Usa un túnel independiente de **Cloudflare** o **[ngrok](https://ngrok.com)**.

   Similar a cómo se pueden conectar los backends de IA, también puedes conectar tu instancia de ST a través de un Cloudflare Tunnel y abrir la página de Cloudflare Tunnel. Sin embargo, tendrás que copiar y pegar cada nuevo enlace generado por Cloudflare/NGROK cada vez que quieras usar ST sobre la marcha.

4. Usa **Tailscale**.

   Tailscale es un proveedor de VPN que permite una conexión remota segura a tu PC.

## Configuración de Tailscale

Tailscale es un proveedor de VPN que permite una conexión remota segura a tu PC. Existe una implementación de código abierto del servidor de Tailscale y también puedes alojar el servidor usando [Headscale](https://github.com/juanfont/headscale), pero eso está fuera del alcance de este tutorial.

### 1. Crear una cuenta

* Ve al [sitio web de Tailscale](https://tailscale.com/) y crea una nueva cuenta.

**NOTA:** Para uso diario por una sola persona, Tailscale será permanentemente gratuito. Si temes costos ocultos, simplemente no agregues ninguna opción de pago.

### 2. Configurar clientes

* Ve a la [página de descargas de Tailscale](https://tailscale.com/download) y descarga el cliente/app en el dispositivo donde tienes SillyTavern ejecutándose y en el dispositivo que deseas usar desde una ubicación remota.
* Inicia sesión en ambos dispositivos con tu cuenta creada anteriormente.
* Ve a la [página de administración de Tailscale](https://login.tailscale.com/admin/machines) y aprueba ambos dispositivos.
* Toma nota de los nombres de ambos dispositivos conectados.

### 3. Agregar tus dispositivos a la lista blanca

* Agrega el nombre de máquina de tu dispositivo de conexión (el que deseas usar con SillyTavern) a la lista blanca de SillyTavern siguiendo [Gestión de IPs en lista blanca](./remote-connections.md#whitelist-based-access-control).

### 4. Conectar

Ahora, siempre que quieras usar SillyTavern desde cualquier lugar, todo lo que tienes que hacer es:

* Tener Tailscale encendido tanto en la PC que aloja SillyTavern como en tu dispositivo que desea usarlo remotamente.
* Abrir un navegador en el dispositivo que desea conectarse e ir a `http://<machine name of PC running st>:8000/`

### 5. Compartir la instancia de SillyTavern con un amigo (opcional)

* Dile a tu amigo que cree su propia cuenta de Tailscale y descargue el cliente en su dispositivo.
* Ve a la [página de administración de Tailscale](https://login.tailscale.com/admin/machines).
* Coloca el cursor sobre el botón de tres puntos en tu PC que aloja SillyTavern y presiona "Share..." o presiona el botón de tres puntos y presiona "Sharing settings...".
* Desmarca "Allow use as an exit node" (a menos que quieras que tu amigo pueda enrutar todo su tráfico de internet a través de tu PC).
* Ya sea envía el enlace como un correo electrónico o cambia la pestaña a "Copy share link", presiona el gran botón azul con el mismo texto y envíaselo a tu amigo de cualquier otra manera.
* Después de hacer clic en tu enlace compartido, tu amigo verá aparecer tu PC en su red de Tailscale.
* Envía a tu amigo el mismo enlace que usas para acceder a SillyTavern como se explicó en el último paso.

**NOTA:** Esto le dará a tu amigo acceso completo a cualquier servicio que se ejecute localmente en tu PC como SillyTavern, automatic1111, etc. Solo haz esto si realmente confías en tu amigo.
