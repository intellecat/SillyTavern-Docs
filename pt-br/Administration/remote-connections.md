---
icon: rss
order: -30
route: /usage/remoteconnections/
---

# Conexões remotas

Na maioria das vezes, isso é para pessoas que desejam usar o SillyTavern em seus telefones celulares enquanto seu PC executa o servidor ST dentro da mesma rede WiFi.

Também é o primeiro passo para permitir conexões remotas de fora da rede local.

!!!warning
Você não deve usar encaminhamento de porta para expor seu servidor ST à internet. Em vez disso, use uma VPN ou um serviço de tunelamento como Cloudflare Zero Trust, ngrok ou Tailscale. Veja o guia [VPN e Tunelamento](tunneling.md) para mais informações.
!!!

!!!danger Aviso
**NUNCA HOSPEDE INSTÂNCIAS NA INTERNET ABERTA SEM GARANTIR MEDIDAS DE SEGURANÇA ADEQUADAS PRIMEIRO.**

**NÃO SOMOS RESPONSÁVEIS POR QUAISQUER DANOS OU PERDAS EM CASOS DE ACESSO NÃO AUTORIZADO DEVIDO À IMPLEMENTAÇÃO DE SEGURANÇA INADEQUADA OU IMPRÓPRIA.**
!!!

## Permitindo conexões remotas

Por padrão, o servidor ST aceita apenas conexões da máquina em que está sendo executado (localhost). Para permitir que ele ouça conexões de outros dispositivos, defina a opção `listen` em `config.yaml` como `true`.

!!! Se você procurar por `config.yaml` diretamente na pasta SillyTavern, você pode encontrar dois arquivos.
Todas as modificações em `config.yaml` neste documento referem-se ao do diretório raiz do SillyTavern (/SillyTavern/config.yaml), e não `/SillyTavern/default/config.yaml`.
!!!

```yaml
# Listen for incoming connections
listen: true
```

Quando o ST estiver ouvindo conexões remotas, você deve ver esta mensagem no console:

```txt
SillyTavern is listening on IPv4: 0.0.0.0:8000
```

e alguma explicação sobre o que isso significa.

Quando o ST **não** estiver ouvindo conexões remotas, você deve ver esta mensagem no console:

```txt
SillyTavern is listening on IPv4: 127.0.0.1:8000
```

## Configuração de controle de acesso

Após habilitar a escuta de conexão remota, você deve configurar pelo menos um método de controle de acesso. Caso contrário, o servidor não iniciará.

### Controle de acesso baseado em whitelist

Para habilitar o controle de acesso via whitelist, edite o arquivo `config.yaml` no diretório raiz do SillyTavern (`/SillyTavern/config.yaml`):

1. Inicie o SillyTavern pelo menos uma vez para gerar os arquivos de configuração necessários.
2. Abra `/SillyTavern/config.yaml` em um editor de texto.
3. Encontre a seção `whitelist` e adicione os endereços IP que deseja permitir:
    * Liste cada endereço IP separadamente.
    * Certifique-se de que `127.0.0.1` esteja incluído, ou você não poderá se conectar da máquina host.
    * Suporta IPs individuais, máscaras CIDR (por exemplo, `10.0.0.0/24`) e intervalos com curinga (`*`).
4. Salve o arquivo `config.yaml`.
5. **Reinicie seu servidor SillyTavern.**

#### Exemplo de configuração de whitelist em `config.yaml`

1. Permitir qualquer dispositivo na rede local:

    ```yaml
    whitelist:
      - ::1
      - 127.0.0.1
      - 10.0.0.0/8
      - 172.16.0.0/12
      - 192.168.0.0/16
    ```

    Se não tiver certeza sobre o intervalo de endereços da sua rede local, use a whitelist acima.

2. Permite dois dispositivos específicos para se conectar:

    ```yaml
    whitelist:
      - ::1
      - 127.0.0.1
      - 192.168.0.2
      - 192.168.0.5
    ```

3. Permite qualquer dispositivo na sub-rede `192.168.0.*` para se conectar:

    ```yaml
    whitelist:
      - ::1
      - 127.0.0.1
      - 192.168.0.*
    ```

4. Permitir conexões de rede para todos os dispositivos IPv4:

    ```yaml
    whitelist:
      - 0.0.0.0/0
    ```

### Desabilitando o controle de acesso baseado em whitelist

Para desabilitar o controle de acesso via whitelist:

* Defina `whitelistMode` como `false` em `/SillyTavern/config.yaml`.
* Remova ou renomeie `whitelist.txt` (se existir) na pasta de instalação base do SillyTavern.
* Reinicie seu servidor SillyTavern.

### Não recomendado: usando `whitelist.txt`

!!!info
Se `whitelist.txt` existir, ele tem precedência sobre as configurações de whitelist em `config.yaml`.

No entanto, como todas as outras configurações são gerenciadas dentro de `config.yaml`, e `whitelist.txt` pode encontrar problemas de permissão ou ficar bloqueado, o sistema pode silenciosamente reverter para usar a whitelist de `config.yaml`.

**Editar config.yaml diretamente é mais simples e mais confiável.**
!!!

Se você ainda preferir usar whitelist.txt:

1. Crie um novo arquivo de texto chamado `whitelist.txt` na pasta de instalação base do SillyTavern.
2. Abra-o em um editor de texto e adicione os endereços IP permitidos.
3. Salve o arquivo e reinicie seu servidor SillyTavern.

#### Exemplo de configuração `whitelist.txt`

```txt
10.0.0.0/8
172.16.0.0/12
192.168.0.0/16
127.0.0.1
::1
```

Isso permite que qualquer dispositivo na rede local se conecte.

### Controle de acesso por autenticação básica HTTP

!!!warning
A autenticação básica HTTP não fornece segurança forte.

Não há limitação de taxa para prevenir ataques de força bruta. Se isso for uma preocupação, é recomendado usar um proxy reverso com TLS e limitação de taxa, e um [serviço de autenticação](sso.md) dedicado.
!!!

O servidor solicitará nome de usuário e senha sempre que um cliente se conectar via HTTP. **Isso só funciona se as conexões remotas (listen: true) estiverem habilitadas.**

Para habilitar HTTP BA, abra `config.yaml` no diretório base do SillyTavern e procure por `basicAuthMode`. Defina basicAuthMode como true e configure nome de usuário e senha. Nota: `config.yaml` só existirá se o ST tiver sido executado antes pelo menos uma vez.

```yaml
basicAuthMode: true
basicAuthUser:
  username: "MyUsername"
  password: "MyPassword"
```

Alternativamente, você pode habilitar autenticação básica da seguinte forma:

```yaml
basicAuthMode: true
enableUserAccounts: true
perUserBasicAuth: true
```

Neste modo `perUserBasicAuth`, o nome de usuário e senha da autenticação básica serão os mesmos de qualquer conta multiusuário válida que tenha uma senha. Além disso, o SillyTavern fará login diretamente nessa conta. **Certifique-se de ter uma conta com senha antes de habilitar `perUserBasicAuth`.**

Salve o arquivo e reinicie o SillyTavern se ele já estiver em execução. Você deverá ser solicitado por nome de usuário e senha ao se conectar ao seu ST. Tanto o nome de usuário quanto a senha são transmitidos em texto simples. Se você estiver preocupado com isso, pode servir o ST via HTTPS.

### Whitelist de host

Ao hospedar um servidor pela rede sem HTTPS, é altamente recomendável habilitar a verificação de host da solicitação. Isso ajuda a prevenir vários ataques, como DNS rebinding. Por padrão, o servidor SillyTavern registrará uma mensagem no console na primeira conexão de um host não reconhecido.

### Alternar whitelist de host

Para habilitar a whitelist de host, edite o arquivo `config.yaml` no diretório raiz do SillyTavern:

```yaml
hostWhitelist:
    enabled: true
```

### Adicionar hosts confiáveis

Para adicionar um nome de host a uma lista de hosts confiáveis, inclua-o na seção `hostWhitelist.hosts`:

!!!tip Dicas
Não adicione `localhost` ou IPs (como `127.0.0.1` ou `::1`). Estes são sempre considerados confiáveis.

Para adicionar um intervalo de hosts, use um ponto inicial. Por exemplo, adicionar `.trycloudflare.com` confiará em `trycloudflare.com` bem como em qualquer subdomínio como `example.trycloudflare.com`.
!!!

```yaml
hostWhitelist:
  hosts:
    - "example.com"
    - ".trycloudflare.com"
```

### Alternar mensagens do console

Para desabilitar mensagens do console para hosts não reconhecidos, defina a opção `hostWhitelist.scan` como `false`:

```yaml
hostWhitelist:
    scan: false
```

## Conectando-se à sua instância do SillyTavern

### Obtendo o endereço IP da máquina host do ST

Após a whitelist ter sido configurada, você precisará do IP do dispositivo que hospeda o ST.

Se o dispositivo que hospeda o ST estiver na mesma rede wifi, você usará o IP wifi interno do host ST:

* Para Windows: botão windows > digite `cmd.exe` na barra de pesquisa > digite `ipconfig` no console, pressione Enter > procure pela listagem `IPv4`.

Se você (ou outra pessoa) quiser se conectar ao seu ST hospedado sem estar na mesma rede, você precisará do IP público do seu dispositivo que hospeda o ST.

* Ao usar o dispositivo que hospeda o ST, acesse [esta página](https://whatismyipaddress.com/) e procure por `IPv4`. Isso é o que você usaria para se conectar do dispositivo remoto.

### Conectando-se ao servidor ST

Qualquer que seja o IP que você obteve para sua situação, você colocará esse endereço IP e número de porta no navegador web do dispositivo remoto.

Um endereço típico para um host ST na mesma rede wifi seria assim:

`http://192.168.0.5:8000`

Use http:// NÃO https://

### Registro de conexões

Novas conexões ao servidor são exibidas na janela do console e registradas no arquivo `access.log` no diretório de dados do SillyTavern.

Uma mensagem do console para um navegador na mesma máquina que o servidor se parece com:

```txt
New connection from 127.0.0.1; User Agent: ...
```

Uma mensagem do console para um navegador em uma máquina diferente na mesma rede que o servidor pode se parecer com:

```txt
New connection from 192.168.116.187; User Agent: ...
```

Se uma conexão for recusada, a mensagem do console será assim:

```txt
New connection from 192.168.116.211; User Agent: ...

Forbidden: Connection attempt from 192.168.116.211. If you are attempting to connect,
please add your IP address in whitelist or disable whitelist mode in config.yaml in
root of SillyTavern folder.
```

`access.log` conterá as informações da conexão, com timestamps, mas não se a conexão foi aceita ou recusada.

### Solução de problemas

Ainda não consegue se conectar?

* Se a tentativa de conexão [aparecer no console](#connection-logging), mas for proibida, é um [problema de whitelist](#whitelist-based-access-control).
* Se o ST estiver ouvindo conexões remotas, mas a tentativa de conexão não aparecer no console, é um [problema de rede](#network-issues).
* Se o ST não estiver ouvindo conexões remotas, é um [problema de leitura](#allowing-remote-connections).

#### Problemas de rede

* No Windows, o aplicativo pode estar bloqueado pelo firewall do aplicativo. A maneira mais rápida de corrigir isso é desinstalar e reinstalar o node.js, e quando solicitado pelo firewall, permitir que ele acesse a rede. Caso contrário, você precisará permitir manualmente o aplicativo node.js através do firewall do aplicativo Windows.
* No Windows 11, habilite o tipo de perfil de Rede Privada em Configurações > Rede e Internet > Ethernet. Isso é MUITO importante para o Windows 11, caso contrário, você não conseguirá se conectar mesmo com as regras de firewall mencionadas acima.
* No Linux, você pode precisar permitir a porta através do firewall. O comando para fazer isso é `sudo ufw allow 8000`. Isso permitirá tráfego na porta 8000.

Não modifique as configurações de encaminhamento de porta no seu roteador. Isso não é necessário para acessar o ST dentro da sua rede local, e pode expor seu servidor à internet.

Se você estiver tentando acessar seu servidor ST de [fora da sua rede local](remote-connections.md), e não estiver funcionando, identifique se o problema está entre o dispositivo remoto e o endpoint do túnel/VPN, ou entre o endpoint do túnel no servidor e o serviço ST. Caso contrário, você passará muito tempo solucionando problemas da coisa errada.

## HTTPS

### Iniciar SillyTavern com TLS/SSL

!!!tip
SSL também pode ser configurado usando o arquivo `config.yaml`: [Configuração SSL](/Administration/config-yaml.md#ssl-configuration).
!!!

Para criptografar o tráfego de e para sua instância ST, inicie o servidor com a flag `--ssl`.

Exemplo:

```bash
node server.js --ssl
```

Por padrão, o ST procurará seus certificados dentro da pasta `certs`. Se seus arquivos estiverem localizados em outro lugar, você pode usar os argumentos `--keyPath` e `--certPath`.

Exemplo:

```bash
node server.js --ssl --keyPath /home/user/certificates/privkey.pem --certPath /home/user/certificates/cert.pem
```

O usuário com o qual você está executando o SillyTavern requer permissões de leitura nos arquivos de certificado.

### Como obter um certificado

A maneira mais simples e rápida de obter um certificado é usando [certbot](https://letsencrypt.org/getting-started/).
