---
label: Single Sign-On (SSO)
icon: key
order: -20
route: /administration/sso/
---

# Single Sign-On (SSO)

SSO permite que você crie usuários e proteja muitas páginas diferentes usando um portal de login apresentado em sites que você deseja proteger. Embora seja complexo de configurar, é uma boa maneira de aprender SSO e proteger sua instância ST na internet com mais segurança.

SSO também pode substituir [Autenticação Básica HTTP](/Administration/config-yaml.md#user-authentication) como um mecanismo de controle de acesso para [conexões remotas](/Administration/remote-connections.md).

Isso é recomendado porque o SSO fornece melhor segurança e funcionalidade do que a Autenticação Básica HTTP.

[**Authelia**](https://www.authelia.com/) e [**Authentik**](https://goauthentik.io/) são provedores de SSO de código aberto que podem ser usados com o SillyTavern.

## Fazer login com SSO

Se o seu nome de usuário fornecido pelo SSO **corresponder exatamente** ao identificador de usuário de uma conta de usuário do SillyTavern, você pode fazer login no SillyTavern como esse usuário por SSO. Para habilitar este recurso, altere uma das seguintes opções no seu arquivo [config.yaml](/Administration/config-yaml.md#sso-auto-login):

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

Ambas as opções aumentam ou substituem o componente de [gerenciamento de senha](/Usage/User_Settings/index.md#account-management) integrado de uma configuração de [modo multiusuário](/Administration/multi-user.md).
