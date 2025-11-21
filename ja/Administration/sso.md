---
label: Single Sign-On (SSO)
icon: key
order: -20
route: /administration/sso/
---

# Single Sign-On (SSO)

SSOを使用すると、ユーザーを作成し、保護したいサイトに表示されるログインポータルを使用して、さまざまなページを保護できます。セットアップは複雑ですが、SSOを学び、インターネット上でSTインスタンスをより安全に保護する良い方法です。

SSOは、[リモート接続](/Administration/remote-connections.md)のアクセス制御メカニズムとして[HTTP Basic Authentication](/Administration/config-yaml.md#user-authentication)を置き換えることもできます。

SSOはHTTP Basic Authenticationよりも優れたセキュリティと機能を提供するため、これが推奨されます。

[**Authelia**](https://www.authelia.com/)と[**Authentik**](https://goauthentik.io/)は、SillyTavernで使用できるオープンソースのSSOプロバイダーです。

## SSOでサインイン

SSO提供のユーザー名がSillyTavernユーザーアカウントのユーザーハンドルと**正確に**一致する場合、SSOによってそのユーザーとしてSillyTavernにサインインできます。この機能を有効にするには、[config.yaml](/Administration/config-yaml.md#sso-auto-login)ファイルに次のオプションのいずれかを変更します:

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

両方のオプションは、[マルチユーザーモード](/Administration/multi-user.md)セットアップの組み込み[パスワード管理](/Usage/User_Settings/index.md#account-management)コンポーネントを拡張または置き換えます。
