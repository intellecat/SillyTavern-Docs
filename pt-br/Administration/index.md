---
order: 25
icon: gear
expanded: true
route: /administration/
---

# Administração

!!!warning
Apesar de seguir muitas práticas recomendadas de segurança, o servidor SillyTavern não é seguro o suficiente para exposição à internet pública.

**NUNCA HOSPEDE INSTÂNCIAS NA INTERNET ABERTA SEM GARANTIR MEDIDAS DE SEGURANÇA ADEQUADAS PRIMEIRO.**

**NÃO SOMOS RESPONSÁVEIS POR QUAISQUER DANOS OU PERDAS RESULTANTES DE ACESSO NÃO AUTORIZADO DEVIDO À IMPLEMENTAÇÃO DE SEGURANÇA INADEQUADA OU IMPRÓPRIA.**
!!!

:::callout
**[config.yaml](./config-yaml.md)**

O arquivo de configuração principal do SillyTavern. Ele contém várias configurações, como rede, segurança e opções específicas de backend.
:::

:::callout
**[Multi-usuário](multi-user)**

Para compartilhar sua instância do SillyTavern com outras pessoas, você pode criar várias contas de usuário. Cada usuário tem suas próprias configurações, extensões e dados. As contas de usuário também podem ser protegidas por senha.
:::

:::callout
**[Acesso remoto](remote-connections)**

Você pode acessar sua instância do SillyTavern do seu telefone, tablet ou outro computador.
:::

:::callout
**[VPNs e Tunelamento](tunneling.md)**

Para acessar sua instância do SillyTavern pela internet, você pode usar uma VPN ou um serviço de tunelamento como Cloudflare Zero Trust, ngrok ou Tailscale.
:::

:::callout
**[Proxy reverso](reverse-proxying)**

Entusiastas podem configurar um proxy reverso para acessar sua instância do SillyTavern pela internet.
:::

## Lista de verificação de segurança

**Estas são apenas recomendações. Por favor, consulte um especialista em segurança de aplicações web antes de tornar sua instância ST pública.**

1. Mantenha seu sistema operacional e software de tempo de execução, como Node.js, atualizados. Isso garante que seu sistema tenha os patches e correções de segurança mais recentes, o que ajuda a prevenir vulnerabilidades potenciais.
2. Use uma [whitelist](/Administration/config-yaml.md#ip-whitelisting) e um firewall de rede. Permita apenas intervalos de IP confiáveis para acessar o servidor.
3. Habilite [autenticação básica](/Administration/config-yaml.md#user-authentication). Ela age como uma "senha mestra" antes que você possa acessar o aplicativo front-end.
4. Alternativamente, configure autenticação externa. Alguns serviços conhecidos para isso são [Authelia](https://www.authelia.com/) e [authentik](https://goauthentik.io/). Veja o [guia SSO](sso.md) para detalhes.
5. Nunca deixe contas de administrador sem senhas. O servidor irá avisá-lo na inicialização se você tiver alguma conta de administrador desprotegida.
6. Use a configuração de login discreto fora da rede local. Isso oculta a lista de usuários de possíveis terceiros.
7. Verifique os logs de acesso com frequência. Eles são gravados no console do servidor e no arquivo `access.log` e fornecem informações sobre conexões recebidas, como endereço IP e user agent.
8. Configure HTTPS. Para um servidor localhost, você pode gerar e usar um certificado autoassinado. Caso contrário, pode ser necessário implantar um servidor web de proxy reverso como [Traefik](https://traefik.io/) ou [Caddy](https://caddyserver.com/docs/getting-started).
9. Configure e habilite [whitelist de host](/Administration/config-yaml.md#host-whitelisting), especialmente se você não estiver usando criptografia HTTPS em uma rede local.

Encontre mais informações sobre proxy seguro no seguinte guia: [Proxy Reverso do SillyTavern](reverse-proxying).
