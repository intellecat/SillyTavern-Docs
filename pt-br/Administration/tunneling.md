---
label: VPNs and Tunneling
order: -40
icon: lock
route: /administration/tunneling/
---

VPNs e túneis são uma maneira segura de acessar sua rede doméstica de qualquer lugar do mundo. Este guia mostrará como usar uma VPN ou um túnel para acessar sua instância do SillyTavern de qualquer lugar.

## Métodos

1. Use uma **VPN caseira**.

   Vários roteadores vêm com a capacidade de hospedar um servidor VPN (principalmente OpenVPN ou WireGuard) na página de administração do roteador. Consulte o manual do seu roteador para configurar uma VPN e adicionar seus dispositivos à VPN. Uma vez conectado, basta ir ao IP privado que você configurou para o SillyTavern e você pode se conectar sem problemas. Mais fácil para usuários e para uso no Windows.

2. Use [**Cloudflare Zero Trust**](https://developers.cloudflare.com/cloudflare-one/).

   Cloudflare Zero Trust é um recurso organizacional gratuito no Cloudflare que permite adicionar 50 usuários. Isso fará proxy do seu tráfego através do Cloudflare e, ao adicionar seu PC ST como um túnel usando `cloudflared`, você pode se conectar à sua instância ST como se estivesse em casa.

   Note que depois de fazer um túnel, você terá que adicionar uma rota para os endereços IP privados do seu roteador e calcular os valores de CIDR de IP para ter acesso local completo em movimento usando Cloudflare Zero Trust.

3. Use um túnel **Cloudflare** ou **[ngrok](https://ngrok.com)** autônomo.

   Similar a como os backends de IA podem se conectar, você também pode conectar sua instância ST via um túnel Cloudflare e abrir a página do túnel Cloudflare. No entanto, você terá que copiar e colar cada novo link gerado pelo Cloudflare/NGROK toda vez que quiser usar o ST em movimento.

4. Use **Tailscale**.

   Tailscale é um provedor de VPN que permite uma conexão remota segura ao seu PC.

## Configuração do Tailscale

Tailscale é um provedor de VPN que permite uma conexão remota segura ao seu PC. Existe uma implementação de código aberto do servidor Tailscale e você também pode hospedar o servidor usando [Headscale](https://github.com/juanfont/headscale), mas isso está fora do escopo deste tutorial.

### 1. Criando uma conta

* Vá ao [site do Tailscale](https://tailscale.com/) e crie uma nova conta.

**NOTA:** Para uso diário por uma única pessoa, o Tailscale será permanentemente gratuito. Se você teme custos ocultos, simplesmente não adicione nenhuma opção de pagamento.

### 2. Configurando clientes

* Vá à [página de download do Tailscale](https://tailscale.com/download) e baixe o cliente/app no dispositivo em que você tem o SillyTavern rodando e no dispositivo que deseja usar de uma localização remota.
* Faça login em ambos os dispositivos com a conta que você criou anteriormente.
* Vá à [página de administração do Tailscale](https://login.tailscale.com/admin/machines) e aprove ambos os dispositivos.
* Anote os nomes de ambos os dispositivos conectados.

### 3. Adicionando seus dispositivos à whitelist

* Adicione o nome da máquina do seu dispositivo de conexão (aquele que você deseja usar o SillyTavern) à whitelist do SillyTavern seguindo [Gerenciando IPs na whitelist](./remote-connections.md#whitelist-based-access-control).

### 4. Conectando

Agora, sempre que você quiser usar o SillyTavern de qualquer lugar, tudo o que você precisa fazer é:

* Ter o Tailscale ligado tanto no PC que hospeda o SillyTavern quanto no seu dispositivo que deseja usá-lo remotamente.
* Abra um navegador no dispositivo que deseja conectar e vá para `http://<machine name of PC running st>:8000/`

### 5. Compartilhando a instância do SillyTavern com um amigo (opcional)

* Diga ao seu amigo para criar sua própria conta Tailscale e baixar o cliente no dispositivo dele.
* Vá à [página de administração do Tailscale](https://login.tailscale.com/admin/machines).
* Passe o mouse sobre o botão de três pontos no seu PC que hospeda o SillyTavern e pressione "Share..." ou pressione o botão de três pontos e pressione "Sharing settings...".
* Desmarque "Allow use as an exit node" (a menos que você queira que seu amigo possa rotear todo o tráfego de internet dele através do seu PC).
* Envie o link como um email ou mude a aba para "Copy share link", pressione o grande botão azul com o mesmo texto e envie para seu amigo de qualquer outra forma.
* Depois de clicar no seu link de compartilhamento, seu amigo verá seu PC aparecer na rede Tailscale dele.
* Envie ao seu amigo o mesmo link que você usa para acessar o SillyTavern conforme explicado no último passo.

**NOTA:** Isso dará ao seu amigo acesso total a quaisquer serviços rodando localmente no seu PC como SillyTavern, automatic1111, etc. Apenas faça isso se você realmente confiar no seu amigo.
