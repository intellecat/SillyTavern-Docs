---
order: 25
icon: gear
expanded: true
route: /administration/
label: Administración
---

# Administración

!!!warning
A pesar de seguir muchas mejores prácticas de seguridad, el servidor de SillyTavern no es lo suficientemente seguro para exponerlo a internet pública.

**NUNCA ALOJES NINGUNA INSTANCIA EN INTERNET ABIERTA SIN ASEGURAR PRIMERO LAS MEDIDAS DE SEGURIDAD APROPIADAS.**

**NO SOMOS RESPONSABLES POR NINGÚN DAÑO O PÉRDIDA RESULTANTE DE ACCESO NO AUTORIZADO DEBIDO A IMPLEMENTACIÓN DE SEGURIDAD INADECUADA O INAPROPIADA.**
!!!

:::callout
**[config.yaml](./config-yaml.md)**

El archivo de configuración principal para SillyTavern. Contiene varias configuraciones, como opciones de red, seguridad y específicas del backend.
:::

:::callout
**[Multi-usuario](multi-user)**

Para compartir tu instancia de SillyTavern con otros, puedes crear múltiples cuentas de usuario. Cada usuario tiene sus propias configuraciones, extensiones y datos. Las cuentas de usuario también pueden estar protegidas con contraseña.
:::

:::callout
**[Acceso remoto](remote-connections)**

Puedes acceder a tu instancia de SillyTavern desde tu teléfono, tableta u otra computadora.
:::

:::callout
**[VPNs y Túneles](tunneling.md)**

Para acceder a tu instancia de SillyTavern desde internet, puedes usar una VPN o un servicio de túneles como Cloudflare Zero Trust, ngrok o Tailscale.
:::

:::callout
**[Proxy inverso](reverse-proxying)**

Los entusiastas pueden configurar un proxy inverso para acceder a su instancia de SillyTavern desde internet.
:::

## Lista de verificación de seguridad

**Estas son solo recomendaciones. Por favor consulta a un especialista en seguridad de aplicaciones web antes de hacer que tu instancia de ST esté disponible públicamente.**

1. Mantén tu sistema operativo y software de ejecución, como Node.js, actualizados. Esto asegura que tu sistema tenga los últimos parches y correcciones de seguridad, lo que ayuda a prevenir vulnerabilidades potenciales.
2. Usa una [lista blanca](/Administration/config-yaml.md#ip-whitelisting) y un firewall de red. Solo permite que rangos de IP confiables accedan al servidor.
3. Habilita la [autenticación básica](/Administration/config-yaml.md#user-authentication). Actúa como una "contraseña maestra" antes de que puedas acceder a la aplicación frontend.
4. Alternativamente, configura autenticación externa. Algunos servicios conocidos para esto son [Authelia](https://www.authelia.com/) y [authentik](https://goauthentik.io/). Consulta la [guía de SSO](sso.md) para más detalles.
5. Nunca dejes cuentas de administrador sin contraseñas. El servidor te advertirá al iniciar si tienes cuentas de administrador desprotegidas.
6. Usa la configuración de inicio de sesión discreto fuera de la red local. Esto oculta la lista de usuarios de posibles extraños.
7. Revisa los registros de acceso con frecuencia. Se escriben en la consola del servidor y en el archivo `access.log` y proporcionan información sobre conexiones entrantes, como dirección IP y agente de usuario.
8. Configura HTTPS. Para un servidor localhost, puedes generar y usar un certificado autofirmado. De lo contrario, es posible que necesites implementar un servidor web de proxy inverso como [Traefik](https://traefik.io/) o [Caddy](https://caddyserver.com/docs/getting-started).
9. Configura y habilita la [lista blanca de host](/Administration/config-yaml.md#host-whitelisting), especialmente si no estás usando cifrado HTTPS en una red local.

Encuentra más información sobre proxy seguro en la siguiente guía: [Proxy inverso de SillyTavern](reverse-proxying).
