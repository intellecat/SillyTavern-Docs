---
route: /extensions/websearch/
---

# Web Search

Adiciona resultados de pesquisa da web aos prompts de LLM.

!!! Note
Algumas fontes de [Chat Completion](/Usage/API_Connections/openai.md) fornecem funcionalidade de pesquisa na web integrada. Neste caso, esta extensão será em grande parte redundante. Verifique o painel **<i class="fa-solid fa-sliders"></i> AI Response Configuration** pela alternância "Enable web search". Por exemplo, isso está disponível para backends Claude, Google AI Studio / Vertex AI, xAI e OpenRouter.
!!!

## Fontes disponíveis

### Selenium Plugin

Requer um plugin oficial do servidor para ser instalado e habilitado.

Veja [SillyTavern-WebSearch-Selenium](https://github.com/SillyTavern/SillyTavern-WebSearch-Selenium) para mais detalhes.

Suporta motores Google e DuckDuckGo.

### Extras API

Requer um módulo `websearch` e navegador web Chrome/Firefox instalado na máquina host.

Suporta motores Google e DuckDuckGo.

### SerpApi

Requer uma chave de API.

Obtenha a chave aqui: <https://serpapi.com/dashboard>

### SearXNG

Requer uma URL de instância SearXNG (privada ou pública). Usa formato HTML para resultados de pesquisa.

String de preferências SearXNG: obtida do SearXNG - preferences - COOKIES - Copy preferences hash

Saiba mais: <https://docs.searxng.org/>

### Tavily AI

Requer uma chave de API.

Obtenha a chave aqui: <https://app.tavily.com/>

### KoboldCpp

A URL do KoboldCpp deve ser fornecida nas configurações de API Text Completion. A versão do KoboldCpp deve ser >= 1.81.1 e o módulo WebSearch deve estar habilitado na inicialização: habilite Network => Enable WebSearch no lançador GUI ou adicione `--websearch` à linha de comando.

Veja: <https://github.com/LostRuins/koboldcpp/releases/tag/v1.81.1>

### Serper

Requer uma chave de API.

Obtenha a chave aqui: <https://serper.dev/>

## Como usar

1. Certifique-se de usar a versão mais recente do SillyTavern.
2. Instale a extensão via menu "Download Extensions & Assets" no SillyTavern.
3. Abra as configurações da extensão "Web Search", defina sua chave de API ou conecte-se ao Extras e habilite a extensão.
4. Os resultados de pesquisa da web serão adicionados ao prompt organicamente enquanto você conversa. **Apenas mensagens do usuário acionam a pesquisa.**
5. Para incluir resultados de pesquisa de forma mais orgânica, envolva consultas de pesquisa com crases simples: ```Tell me about the `latest Ryan Gosling movie`.``` produzirá uma consulta de pesquisa `latest Ryan Gosling movie`.
6. Opcionalmente, configure as configurações de acordo com sua preferência.

## Configurações

### Geral

1. Enabled - liga e desliga a extensão.
2. Sources = define a fonte de resultados de pesquisa.
3. Cache Lifetime - por quanto tempo (em segundos) os resultados de pesquisa são armazenados em cache para seu prompt. Padrão = uma semana.

### Configurações de Prompt

1. Prompt Budget - define a capacidade máxima do texto inserido (em caracteres de texto, NÃO tokens). Regra geral: 1 token ~ 3-4 caracteres, ajuste de acordo com os limites de contexto do seu modelo. Padrão = 1500 caracteres.
2. Insertion Template - como o resultado é inserido no prompt. Suporta as macros usuais + macro especial: \{\{query\}\} para consulta de pesquisa e \{\{text\}\} para resultados de pesquisa.
3. Injection Position - onde o resultado vai no prompt. As mesmas opções que para Author's Note: como injeção no chat ou antes/depois do prompt do sistema.

### Ativação de Pesquisa

1. Use function tool - usa [chamada de função](/For_Contributors/Function-Calling.md) para ativar pesquisa ou raspar páginas da web. Deve usar uma API Chat Completion suportada e estar habilitado nas configurações de AI Response. **Desabilita todos os outros métodos de ativação quando ativo.**
2. Use Backticks - habilita a ativação de pesquisa usando palavras envoltas em crases simples.
3. Use Trigger Phrases - habilita a ativação de pesquisa usando frases de gatilho.
4. Regular expressions - forneça um regex com sabor JS para corresponder à mensagem do usuário. Se o regex corresponder, a pesquisa com uma determinada consulta será acionada. A consulta de pesquisa suporta `{{macros}}` e sintaxe $1 para referenciar o grupo correspondido. Exemplo: `/what is happening in (.*)/i` regex para consulta de pesquisa `news in $1` corresponderá a uma mensagem contendo `what is happening in New York` e acionará a pesquisa com a consulta `news in New York`.
5. Trigger Phrases - adicione frases que acionarão a pesquisa, uma por uma. Pode estar em qualquer lugar na mensagem, e a consulta começa da palavra de gatilho e se estende até "Max Words" no total. Para excluir uma mensagem específica do processamento, ela deve começar com um ponto, por exemplo `.What do you think?`. Prioridade dos gatilhos: primeiro por ordem na caixa de texto, depois o primeiro na mensagem do usuário.
6. Max Words - quantas palavras são incluídas na consulta de pesquisa (incluindo a frase de gatilho). O Google tem um limite de cerca de 32 palavras por prompt. Padrão = 10 palavras.

### Raspagem de Página

1. Visit Links - o texto será extraído das páginas de resultados de pesquisa visitadas e salvo em um anexo de arquivo.
2. Visit Count - quantos links serão visitados e analisados para texto.
3. Visit Domain Blacklist - domínios de sites a serem excluídos da visita. Um por linha.
4. File Header - template de cabeçalho de arquivo, inserido no início do arquivo de texto, tem uma macro adicional \{\{query\}\}.
5. Block Header - template de bloco de link, inserido com o conteúdo analisado de cada link. Use a macro \{\{link\}\} para URL da página e \{\{text\}\} para conteúdo da página.
6. Save Target - onde salvar os resultados da raspagem. Opções possíveis: anexos de mensagem de gatilho, ou anexos de chat do Data Bank, ou apenas imagens (se a fonte suportar).
7. Include Images - anexar imagens relevantes ao chat. Requer uma fonte que suporte imagens (veja abaixo).

## Mais informações

Os resultados de pesquisa da consulta mais recente permanecerão incluídos no prompt até que a próxima consulta válida seja encontrada.
Se você quiser fazer perguntas adicionais sem acidentalmente acionar a pesquisa, comece sua mensagem com um ponto.

!!!info
A ferramenta de função Web Search sempre substitui outros gatilhos se habilitada e disponível.
!!!

Prioridade de gatilhos (se múltiplos estiverem habilitados):

1. Crases.
2. Expressões regulares.
3. Frases de gatilho.

Para descartar todas as consultas anteriores do processamento, comece a mensagem do usuário com um ponto de exclamação, por exemplo, uma mensagem do usuário `!Now let's talk about...` descartará esta e todas as mensagens acima dela.

Esta extensão também fornece um comando slash `/websearch` para usar em STscript. Mais informações aqui: [STscript Language Reference](/For_Contributors/st-script.md#extension-commands)

```stscript
/websearch (links=on|off snippets=on|off [query]) – performs a web search query. Use named arguments to specify what to return - page snippets (default: on), full parsed pages (default: off) or both.

Example: /websearch links=off snippets=on how to make a sandwich
```

### O que pode ser incluído no resultado de pesquisa?

**Glossário:**

- Answer box: Resposta direta à pergunta.
- Knowledge graph: Conhecimento enciclopédico sobre o tópico.
- Page snippets: Extratos relevantes das páginas da web.
- Relevant questions: Perguntas e respostas de tópicos similares.
- Images: Imagens relevantes.

#### SerpApi

1. Answer box.
2. Knowledge graph.
3. Page snippets (máx 10).
4. Relevant questions (máx 10).
5. Images (máx 10).

#### Selenium Plugin e Extras API

1. Google - answer box, knowledge graph, page snippets.
2. DuckDuckGo - page snippets.

**Selenium Plugin** pode adicionalmente fornecer imagens.

#### SearXNG

1. Infobox.
2. Page snippets.
3. Images.

#### Tavily AI

1. Answer.
2. Page contents.
3. Images (até 5).

#### KoboldCpp

1. Page titles.
2. Page snippets.

#### Serper

1. Answer box.
2. Knowledge graph.
3. Page snippets.
4. Relevant questions.
5. Images.
