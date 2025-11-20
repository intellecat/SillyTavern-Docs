---
order: 30
route: /usage/core-concepts/data-bank/
tags:
    [
        vector storage,
        RAG,
        retrieval-augmented generation,
        vectors,
        documents,
        files,
        attachments,
    ]
---

# Data Bank (RAG)

Retrieval-augmented generation (RAG) é uma técnica para fornecer fontes externas de conhecimento ao LLM. Ajuda a melhorar a precisão das respostas da IA acessando informações fora dos dados de treinamento do modelo.

O SillyTavern fornece um conjunto de ferramentas para construir uma base de conhecimento multiuso a partir de diversas fontes, bem como usar os dados coletados em prompts de LLM.

## Acessando o Data Bank

A extensão integrada Chat Attachments (incluída por padrão em versões de lançamento >= 1.12.0) adiciona uma nova opção no menu "Magic Wand" - Data Bank. Este é o seu hub para gerenciar os documentos disponíveis para RAG no SillyTavern.

## Sobre Documentos

O Data Bank armazena anexos de arquivo, também conhecidos como documentos. Os documentos são divididos em três escopos de disponibilidade.

1. Global attachments - disponíveis em todos os chats, seja solo ou em grupo.
2. Character attachments - disponíveis apenas para o personagem atualmente escolhido, incluindo quando eles estão respondendo em um grupo. _Anexos são salvos localmente e não são exportados com o card do personagem!_
3. Chat attachments - disponíveis apenas no chat atualmente aberto. Cada personagem no chat pode extrair dele.

!!!info Nota
Embora não seja formalmente parte do data bank, você pode anexar arquivos até mesmo a mensagens individuais. Use a opção Attach File do menu "Wand", ou um ícone de clipe de papel na linha de ações da mensagem.
!!!

O que pode ser um documento? Praticamente qualquer coisa que seja representável em forma de texto simples!

Exemplos incluem, mas não se limitam a:

- Arquivos locais (livros, artigos científicos, etc.)
- Páginas da web (Wikipedia, artigos, notícias)
- Transcrições de vídeo

Várias extensões e plugins também podem fornecer novas maneiras de coletar e processar dados, mais sobre isso abaixo.

## Fontes de Dados

Para adicionar um documento a qualquer um dos escopos, clique em "Add" e escolha uma das fontes disponíveis.

### Notepad

Crie um arquivo de texto do zero ou edite um anexo existente.

### File

Faça upload de um arquivo do disco rígido do seu computador. O SillyTavern fornece conversores integrados para formatos de arquivo populares:

- PDF (apenas texto)
- HTML
- Markdown
- ePUB
- TXT

Você também pode anexar quaisquer arquivos de texto com extensões não padrão, como JSON, YAML, códigos-fonte, etc. Se não houver conversões conhecidas do tipo de arquivo selecionado, e o arquivo não puder ser analisado como um documento de texto simples, o upload do arquivo será rejeitado, o que significa que arquivos binários brutos não são permitidos.

!!!info Nota
Importar documentos do Microsoft Office (DOCX, PPTX, XLSX) e LibreOffice (ODT, ODP, ODS) requer um [Server Plugin](https://github.com/SillyTavern/SillyTavern-Office-Parser) para ser instalado e carregado. Veja a página README do plugin para instruções de instalação.
!!!

### Web

Extraia texto de uma página da web por sua URL. O documento HTML é então processado através da biblioteca [Readability](https://github.com/mozilla/readability) para extrair apenas texto utilizável.

Alguns servidores web podem rejeitar solicitações de busca, estar protegidos pelo Cloudflare ou depender fortemente de JavaScript para funcionar. Se você estiver enfrentando problemas com algum site específico, baixe a página manualmente através do navegador web e anexe-a usando o uploader de arquivo.

### YouTube

Baixe uma transcrição do vídeo do YouTube por seu ID ou URL, seja enviada pelo criador ou gerada automaticamente pelo Google. Alguns vídeos podem ter as transcrições desabilitadas, também a análise de vídeos com restrição de idade não está disponível, pois requer login.

O script é carregado no idioma padrão do vídeo. Opcionalmente, você pode especificar o código de idioma de duas letras para tentar buscar a transcrição em um idioma específico. Este recurso nem sempre está disponível e pode falhar, então use-o com cautela.

### Web Search

!!!info Nota
Esta fonte requer ter uma extensão [Web Search](/extensions/WebSearch.md) instalada e configurada corretamente. Veja a página vinculada para mais detalhes.
!!!

Realize uma pesquisa na web e baixe o texto das páginas de resultados de pesquisa. Isso é similar à fonte Web, mas totalmente automatizada. Um mecanismo de busca escolhido será herdado das configurações da extensão, então configure-o com antecedência.

Para começar, especifique a consulta de pesquisa, número máximo de links a serem visitados e o tipo de saída: um arquivo combinado (formatado de acordo com as regras da extensão) ou um arquivo individual para cada página. Você pode escolher salvar os snippets da página também.

### Fandom

!!!info Nota
Esta fonte requer ter um [Server Plugin](https://github.com/SillyTavern/SillyTavern-Fandom-Scraper) instalado e carregado. Veja a página README do plugin para instruções de instalação.
!!!

Extraia artigos de uma wiki [Fandom](https://www.fandom.com/) por seu ID ou URL. Como algumas wikis são muito grandes, pode ser benéfico limitar o escopo usando a expressão regular de filtro, ela será testada contra o título do artigo. Se nenhum filtro for fornecido, então todas as páginas estão sujeitas a serem exportadas. Você pode salvá-las como arquivos individuais para cada página, ou juntas em um único documento.

### Bronie Parser Extension (Third-Party)

!!!warning Nota
Esta fonte vem de terceiros e **não é afiliada** à equipe SillyTavern. Esta fonte requer que você tenha a [Bronie Parser Extension](https://github.com/Bronya-Rand/Bronie-Parser-Extension) de Bronya Rand instalada, bem como Server Plugins que requerem o parser para funcionar.
!!!

A Bronie Parser Extension de Bronya Rand permite o uso de scrapers de terceiros, como o [HoYoLab](https://wiki.hoyolab.com) da miHoYo/HoYoverse no SillyTavern, similar às outras fontes de dados.

Atualmente, a Bronie Parser Extension de Bronya Rand suporta o seguinte:

- HoYoLab da miHoYo/HoYoverse (para Genshin Impact/Honkai: Star Rail) via [HoYoWiki-Scraper-TS](https://github.com/Bronya-Rand/HoYoWiki-Scraper-TS)

Para começar, instale a Bronie Parser Extension de Bronya Rand seguindo seu [guia de instalação](https://github.com/Bronya-Rand/Bronie-Parser-Extension?tab=readme-ov-file#installation) e instale um Server Plugin suportado no SillyTavern. Reinicie o SillyTavern e vá para o menu _Data Bank_. Clique em `+ Add` e você deve ver que seus scrapers recentemente instalados são adicionados à lista possível de fontes para obter informações.

## Vector Storage

Então, você construiu para si mesmo uma biblioteca boa e abrangente de informações sobre seu assunto específico. E agora?

Para usar os documentos para RAG, você precisa usar uma extensão compatível que inserirá dados relacionados no prompt do LLM.

Vector Storage, que vem junto com o SillyTavern, é uma implementação de referência de tal extensão. Usa embeddings (também conhecidos como vetores) para pesquisar documentos que se relacionam com seus chats em andamento.

!!!info Curiosidades

1. Embeddings são arrays de números que representam abstratamente um pedaço de texto, produzidos por modelos de linguagem especializados. Textos mais similares têm uma distância menor entre seus respectivos vetores.
2. A extensão Vector Storage usa a biblioteca [Vectra](https://github.com/Stevenic/vectra) para acompanhar os embeddings de arquivo. Eles são armazenados em arquivos JSON na pasta `/vectors` do seu diretório de dados do usuário. Cada documento é representado internamente por seu próprio arquivo de índice/coleção.
   !!!

Como a funcionalidade Vectors está desabilitada por padrão, você precisa abrir o painel de extensões (ícone "Stacked Cubes" na barra superior), depois navegar até a seção "Vector Storage" e marcar a caixa de seleção "Enabled for files" em "File vectorization settings".

Por si só, Vector Storage não produz nenhum vetor, você precisa usar um provedor de embedding compatível.

## Vector Providers

!!!warning Aviso
Embeddings são utilizáveis apenas quando são recuperados usando o mesmo modelo que os gerou. Ao alterar um modelo ou fonte de embedding, os vetores precisam ser recalculados.
!!!

### Local

Essas fontes são gratuitas e ilimitadas e usam sua CPU/GPU para calcular embeddings.

1. Local (Transformers) - executa em um servidor Node. O SillyTavern baixará automaticamente um modelo compatível em formato ONNX do HuggingFace. Modelo padrão: [jina-embeddings-v2-base-en](https://huggingface.co/Cohee/jina-embeddings-v2-base-en).
2. WebLLM - requer que uma extensão seja instalada e um navegador web que [suporte WebGPU](https://caniuse.com/webgpu). Executa diretamente em seu navegador, pode usar aceleração de hardware. Baixa automaticamente modelos suportados do HuggingFace. Instale a extensão daqui: <https://github.com/SillyTavern/Extension-WebLLM>.
3. Ollama - obtenha em <https://ollama.com/>. Defina a URL da API no menu de conexão da API (em Text Completion, padrão: `http://localhost:11434`). Deve baixar um modelo compatível primeiro, depois definir seu nome nas configurações da extensão. Modelo exemplo: [mxbai-embed-large](https://ollama.com/library/mxbai-embed-large). Opcionalmente, marque uma opção para manter o modelo carregado na memória.
4. llama.cpp server - obtenha de [ggerganov/llama.cpp](https://github.com/ggerganov/llama.cpp) e execute o executável do servidor com a flag `--embedding`. Carregue modelos de embedding GGUF compatíveis do HuggingFace, por exemplo, [nomic-ai/nomic-embed-text-v1.5-GGUF](https://huggingface.co/nomic-ai/nomic-embed-text-v1.5-GGUF).
5. vLLM - obtenha de [vllm-project/vllm](https://github.com/vllm-project/vllm). Defina a URL da API e chave de API no menu de conexão da API primeiro.
6. Extras (obsoleto) - executa sob a [Extras API](https://github.com/SillyTavern/SillyTavern-extras) usando o loader SentenceTransformers. Modelo padrão: [all-mpnet-base-v2](https://huggingface.co/sentence-transformers/all-mpnet-base-v2). Esta fonte não é mantida e será eventualmente removida no futuro.

### Fontes de API

Todas essas fontes requerem uma chave de API do respectivo serviço e geralmente têm um custo de uso, mas geralmente calcular embeddings é bastante barato.

1. OpenAI
2. Cohere
3. Google AI Studio
4. Google Vertex AI
5. TogetherAI
6. MistralAI
7. NomicAI

## Configurações de Vetorização

Depois de selecionar seu provedor de embedding, não se esqueça de configurar outras configurações que definirão as regras para processar e recuperar documentos.

!!!info Nota
Dividir, vetorizar e recuperar informações dos anexos leva algum tempo. Embora a ingestão inicial do arquivo possa demorar um pouco, as consultas de pesquisa RAG geralmente são rápidas o suficiente para não criar um atraso significativo.
!!!

### Message attachments

Essas configurações controlam os arquivos que são anexados diretamente às mensagens.

As seguintes regras se aplicam:

1. Apenas mensagens que cabem na janela de contexto do LLM podem ter seus anexos recuperados.
2. Quando a extensão vector storage está desabilitada, anexos de arquivo e sua mensagem acompanhante são totalmente inseridos no prompt.
3. Quando a vetorização de arquivo está habilitada, então o arquivo será dividido em chunks e apenas as partes mais relevantes serão inseridas, economizando o espaço de contexto e permitindo que o modelo permaneça focado.

- Size threshold (KB) - define um limite de divisão de chunking. Apenas os arquivos maiores que o tamanho especificado serão divididos.
- Chunk size (chars) - define o tamanho alvo de um chunk individual (em caracteres textuais, não tokens de modelo!).
- Chunk overlap (%) - define a porcentagem de um tamanho de chunk que será compartilhado entre chunks adjacentes. Isso permite uma transição mais suave entre os chunks, mas também pode introduzir alguma redundância.
- Retrieve chunks - define a quantidade máxima dos chunks de arquivo mais relevantes a serem recuperados. Eles serão inseridos em sua ordem original.

### Data Bank files

Essas configurações controlam como os documentos do Data Bank são tratados.

As seguintes regras se aplicam:

1. Quando a vetorização de arquivo está desabilitada, o Data Bank não é usado.
2. Caso contrário, todos os documentos disponíveis do escopo atual (veja acima) são considerados para a consulta. Apenas os chunks mais relevantes em todos os arquivos são recuperados. Vários chunks do mesmo arquivo são inseridos em sua ordem original.
3. Os chunks inseridos reservarão uma parte do contexto antes de ajustar as mensagens de chat.

- Size threshold (KB) - define um limite de divisão de chunking. Apenas os arquivos maiores que o tamanho especificado serão divididos.
- Chunk size (chars) - define o tamanho alvo de um chunk individual (em caracteres textuais, não tokens de modelo!).
- Chunk overlap (%) - define a porcentagem de um tamanho de chunk que será compartilhado entre chunks adjacentes. Isso permite uma transição mais suave entre os chunks, mas também pode introduzir alguma redundância.
- Retrieve chunks - define a quantidade máxima de chunks de arquivo a serem recuperados. Essa permissão é compartilhada entre todos os arquivos.
- Injection Template - define como a informação recuperada será inserida no prompt. Você pode usar um macro especial \{\{text\}\} para especificar a posição do texto recuperado, bem como quaisquer outros macros.
- Injection Position - define onde inserir a injeção de prompt. As mesmas regras que para Author's Note e World Info se aplicam.

### Configurações compartilhadas

- Query messages - quantas das últimas mensagens de chat serão usadas para consultar chunks de documento.
- Score threshold - ajuste para permitir a eliminação da recuperação de chunks com base em sua pontuação de relevância (0 - sem correspondência, 1 - correspondência perfeita). Valores mais altos permitem recuperação mais precisa e impedem que informações completamente aleatórias entrem no contexto. Valores sãos estão em uma faixa entre 0.2 (mais solto) e 0.5 (mais focado).
- Chunk boundary - uma string personalizada que será priorizada ao dividir os arquivos em chunks. Se não especificado, o padrão é dividir por (em ordem) quebras de linha duplas, quebras de linha simples e espaços entre palavras.
- Only chunk on custom boundary - se habilitado, o chunking ocorrerá apenas no limite de chunk especificado. Caso contrário, o chunking também ocorrerá nos limites padrão.
- Translate files into English before processing - se habilitado, usará a API de tradução configurada na extensão [Chat Translation](/extensions/Translation.md) para traduzir os arquivos para o inglês antes de processá-los. Isso é útil ao usar modelos de embedding que suportam apenas texto em inglês.
- Include in World Info Scanning - marque se você deseja que o conteúdo injetado ative entradas de lore book.
- Vectorize All - ingere forçadamente os embeddings para todos os arquivos não processados.
- Purge Vectors - limpa os embeddings de arquivo, permitindo recalcular seus vetores.

!!!info Nota
Para configurações de "Chat vectorization", veja [Chat Vectorization](/extensions/Chat-vectorization.md).
!!!

## Conclusão

Parabéns! Sua experiência de chat agora está aprimorada com o poder do RAG. Suas capacidades são limitadas apenas pela sua imaginação. Como sempre, não tenha medo de experimentar!
