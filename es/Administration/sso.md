---
label: Inicio de sesión único (SSO)
icon: key
order: -20
route: /administration/sso/
---

# Inicio de sesión único (SSO)

SSO le permite crear usuarios y asegurar muchas páginas diferentes utilizando un portal de inicio de sesión presentado en los sitios que desea proteger. Aunque es complejo de configurar, es una buena manera tanto de aprender SSO como de asegurar mejor su instancia de ST en Internet.

SSO también puede reemplazar [HTTP Basic Authentication](/Administration/config-yaml.md#user-authentication) como mecanismo de control de acceso para [conexiones remotas](/Administration/remote-connections.md).

Esto se recomienda porque SSO proporciona mejor seguridad y funcionalidad que HTTP Basic Authentication.

[**Authelia**](https://www.authelia.com/) y [**Authentik**](https://goauthentik.io/) son proveedores de SSO de código abierto que se pueden usar con SillyTavern.

## Iniciar sesión con SSO

Si su nombre de usuario proporcionado por SSO coincide **exactamente** con el identificador de usuario de una cuenta de usuario de SillyTavern, puede iniciar sesión en SillyTavern como ese usuario mediante SSO. Para habilitar esta característica, cambie una de las siguientes opciones en su archivo [config.yaml](/Administration/config-yaml.md#sso-auto-login):

### Authelia

```yaml
sso:
  autheliaAuth: true
```

### Authentik

```yaml
sso:
  authentikAuth: true
```

Ambas opciones aumentan o reemplazan el componente integrado de [gestión de contraseñas](/Usage/User_Settings/index.md#account-management) de una configuración en [modo multiusuario](/Administration/multi-user.md).
