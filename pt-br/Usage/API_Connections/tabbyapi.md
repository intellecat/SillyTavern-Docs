---
route: /usage/api-connections/tabbyapi/
---

# TabbyAPI
Uma aplicação baseada em FastAPI que permite gerar texto usando um LLM com o backend Exllamav2, com suporte para modelos Exl2, GPTQ e FP16.

* [GitHub](https://github.com/theroyallab/tabbyAPI)

### Início Rápido
1. Siga as [instruções de instalação](https://github.com/theroyallab/tabbyAPI/wiki/01.-Getting-Started) no GitHub oficial do TabbyAPI.
2. [Crie seu config.yml](https://github.com/theroyallab/tabbyAPI/wiki/02.-Server-options) para definir o caminho do seu modelo, modelo padrão, comprimento de sequência, etc. Você pode ignorar a maioria (se não todas) dessas configurações se desejar.
3. Inicie o TabbyAPI. Se funcionou, você deverá ver algo assim:

    ![TabbyAPI terminal](/static/tabby-terminal.png)

4. Em Text Completion API no SillyTavern, selecione TabbyAPI.
5. Copie sua chave de API do terminal do TabbyAPI para `Tabby API key` e certifique-se de que seu `API URL` está correto (deve ser `http://127.0.0.1:5000` por padrão).

Se você fez tudo corretamente, deverá ver algo assim no SillyTavern:

![TabbyAPI SillyTavern](/static/tabby-config.png)

Agora você pode conversar usando TabbyAPI!

### TabbyAPI Loader
Os desenvolvedores do TabbyAPI criaram uma extensão oficial para carregar/descarregar modelos diretamente do SillyTavern. A instalação é simples:
1. No SillyTavern, clique na aba Extensions e navegue até Download Extensions & Assets.
2. Copie `https://raw.githubusercontent.com/theroyallab/ST-repo/main/index.json` em Assets URL e clique no botão de plug à direita.
3. Você deverá ver algo assim. Clique no botão de download ao lado de Tabby Loader.

    ![Tabby Loader](/static/tabby-assets.png)

4. Se a instalação foi bem-sucedida, você deverá ver uma mensagem pop-up verde no topo da sua tela. Na aba extensions, navegue até TabbyAPI Loader e copie sua chave de administrador do terminal do TabbyAPI para Admin Key.
5. Clique no botão de atualização ao lado de Model Select. Quando você clicar na caixa de texto logo abaixo, deverá ver todos os modelos no seu diretório de modelos.

![Tabby Loader Extension](/static/tabby-loader.png)

Agora você pode carregar e descarregar seus modelos diretamente do SillyTavern!

### Suporte
Ainda precisa de ajuda? Visite o [GitHub do TabbyAPI](https://github.com/theroyallab/tabbyAPI) para um link ao servidor Discord oficial do desenvolvedor e [leia a wiki](https://github.com/theroyallab/tabbyAPI/wiki/1.-Getting-Started).
