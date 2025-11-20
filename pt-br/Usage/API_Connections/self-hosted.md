---
order: 90
icon: desktop-download
route: /usage/how-to-use-a-self-hosted-model/
---

# Modelos de IA auto-hospedados

!!!warning
Este guia é baseado na experiência e conhecimento pessoal do autor e não é uma verdade absoluta. Todas as declarações devem ser tomadas com cautela. Se você tiver correções ou sugestões, entre em contato conosco no Discord ou envie um PR para o [repositório de documentação do SillyTavern](https://github.com/SillyTavern/SillyTavern-Docs).
!!!

## Introdução

Este guia tem como objetivo ajudá-lo a configurar o SillyTavern com uma IA local rodando no seu PC (vamos começar a usar a terminologia apropriada a partir de agora e chamá-la de LLM). Leia-o antes de incomodar as pessoas com perguntas de suporte técnico.

### Quais são os melhores Large Language Models?

É impossível responder a essa pergunta, pois não existe uma escala padronizada de "Melhor". A comunidade tem recursos e discussões suficientes acontecendo no Reddit e Discord para formar pelo menos alguma opinião sobre qual é o modelo preferido/principal. Sua experiência pode variar.

### Qual é a melhor configuração?

Se houvesse uma melhor configuração ou uma configuração óbvia, haveria necessidade de configuração? A melhor configuração é aquela que funciona para você. É um processo de tentativa e erro.

## Requisitos de hardware e orientação

Este é um assunto complexo, então vou me ater ao essencial e generalizar.

* Existem milhares de LLMs gratuitos que você pode baixar da Internet, semelhante a como o Stable Diffusion tem toneladas de modelos que você pode obter para gerar imagens.
* Executar um LLM não modificado requer uma GPU monstruosa com toneladas de VRAM (memória da GPU). Mais do que você jamais terá.
* É possível reduzir os requisitos de VRAM comprimindo o modelo usando técnicas de quantização, como GPTQ ou AWQ. Isso torna o modelo um pouco menos capaz, mas reduz bastante os requisitos de VRAM para executá-lo. De repente, isso permitiu que pessoas com GPUs para jogos como uma 3080 executassem um modelo 13B. Mesmo que não seja tão bom quanto o modelo não quantizado, ainda é bom.
* Fica melhor: também existe um formato de modelo e quantização chamado GGUF (anteriormente GGML) que se tornou o formato de escolha para pessoas normais sem GPUs monstruosas. Isso permite que você use um LLM sem uma GPU. Ele usará apenas CPU e RAM. Isso é muito mais lento (provavelmente 15 vezes) do que executar o LLM em uma GPU usando GPTQ/AWQ, especialmente durante o processamento do prompt, mas a capacidade do modelo é igualmente boa. O criador do GGUF então otimizou o GGUF ainda mais adicionando uma opção de configuração que permite que pessoas com uma GPU de nível para jogos transfiram partes do modelo para a GPU, permitindo que executem parte do modelo na velocidade da GPU (observe que isso não reduz os requisitos de RAM, apenas melhora sua velocidade de geração).
* Existem diferentes tamanhos de modelos, nomeados com base no número de parâmetros com os quais foram treinados. Você verá nomes como 7B, 13B, 30B, 70B, etc. Você pode pensar neles como o tamanho do cérebro do modelo. Um modelo 13B será mais capaz que o 7B da mesma família de modelos: eles foram treinados nos mesmos dados, mas o cérebro maior pode reter o conhecimento melhor e pensar de forma mais coerente. Modelos maiores também exigem mais VRAM/RAM.
* Existem vários graus de quantização (8-bit, 5-bit, 4-bit, etc). Quanto mais baixo você vai, mais o modelo se degrada, mas menores são os requisitos de hardware. Então, mesmo em hardware ruim, você pode conseguir executar uma versão de 4-bit do modelo desejado. Existe até quantização de 3-bit e 2-bit, mas neste ponto, você está batendo em cavalo morto. Também existem subtipos adicionais de quantização nomeados k_s, k_m, k_l, etc. k_m é melhor que k_s, mas requer mais recursos.
* O tamanho do contexto (quanto tempo sua conversa pode se tornar sem o modelo descartar partes dela) também afeta os requisitos de VRAM/RAM. Felizmente, esta é uma configuração ajustável, permitindo que você use um contexto menor para reduzir os requisitos de VRAM/RAM. (Nota: o tamanho de contexto de modelos baseados em Llama2 é 4k. Mistral é anunciado como 8k, mas é 4k na prática.)
* Em algum momento de 2023, a NVIDIA mudou seu driver de GPU para que, se você precisar de mais VRAM do que sua GPU tem, em vez da tarefa travar, ela começará a usar a RAM regular como alternativa. Isso arruinará a velocidade de escrita do LLM, mas o modelo ainda funcionará e dará a mesma qualidade de saída. Felizmente, esse comportamento [pode ser desativado](https://nvidia.custhelp.com/app/answers/detail/a_id/5490).

Dado tudo o acima, os requisitos de hardware e desempenho variam completamente dependendo da família do modelo, do tipo de modelo, do tamanho do modelo, do método de quantização, etc.

#### Calculadora de tamanho de modelo
Você pode usar a [Calculadora de Tamanho de Modelo da Nyx](https://huggingface.co/spaces/NyxKrage/LLM-Model-VRAM-Calculator) para determinar quanta RAM/VRAM você precisa.

Lembre-se, você deseja executar o maior modelo, menos quantizado, que caiba na sua memória, ou seja, sem causar [troca de disco](https://serverfault.com/a/48487).

## Baixando um LLM

Para começar, você precisará baixar um LLM. O lugar mais comum para encontrar e baixar LLMs é no HuggingFace. Existem milhares de modelos disponíveis. Uma boa maneira de encontrar modelos GGUF é verificar a página da conta do bartowski: <https://huggingface.co/bartowski>. Se você não quer GGUF, ele vincula a página do modelo original onde você pode encontrar outros formatos para esse mesmo modelo.

Na página de um determinado modelo, você encontrará um monte de arquivos.

* Você pode não precisar de todos eles! Para GGUF, você só precisa do arquivo de modelo .gguf (geralmente 4-11GB). Se você encontrar vários arquivos grandes, geralmente são todas quantizações diferentes do mesmo modelo, você só precisa escolher um.
* Para arquivos .safetensors (que podem ser GPTQ ou AWQ ou HF quantizado ou não quantizado), se você vir uma sequência numérica no nome do arquivo como model-00001-of-00003.safetensors, então você precisa de todos os 3 desses arquivos .safetensors + todos os outros arquivos no repositório (tokenizer, configs, etc.) para obter o modelo completo.
* A partir de janeiro de 2024, Mixtral MOE 8x7B é amplamente considerado o estado da arte para LLMs locais. Se você tem os 32GB de RAM para executá-lo, definitivamente experimente. Se você tem menos de 32GB de RAM, então use Kunoichi-DPO-v2-7B, que apesar de seu tamanho é estelar desde o início.

### Passo a passo para baixar Kunoichi-DPO-v2-7B

Usaremos o modelo Kunoichi-DPO-v2-7B para o resto deste guia. É um excelente modelo baseado em Mistral 7B, que requer apenas 7GB de RAM e está muito acima de seu peso. Nota: Kunoichi usa prompting Alpaca.

* Vá para <https://huggingface.co/brittlewis12/Kunoichi-DPO-v2-7B-GGUF>
* Clique em 'Files and versions'. Você verá uma lista de vários arquivos. Estes são todos o mesmo modelo, mas oferecidos em diferentes opções de quantização. Clique no arquivo 'kunoichi-dpo-v2-7b.Q6_K.gguf', que nos dá uma quantização de 6-bit.
* Clique no botão 'download'. Seu download deve começar.

### Como identificar o tipo de modelo

Bons uploaders de modelo como TheBloke dão nomes descritivos. Mas se não derem:

* Nome do arquivo termina em .gguf: modelo GGUF CPU (obviamente)
* Nome do arquivo termina em .safetensors: pode ser não quantizado, ou HF quantizado, ou GPTQ, ou AWQ
* Nome do arquivo é pytorch-***.bin: mesmo que acima, mas este é um formato de arquivo de modelo mais antigo que permite ao modelo executar script Python arbitrário quando o modelo é carregado, e é considerado inseguro. Você ainda pode usá-lo se confiar no criador do modelo, ou estiver desesperado, mas escolha .safetensors se tiver a opção.
* config.json existe? Veja se ele tem um quant_method.
* q4 significa quantização de 4-bit, q5 é quantização de 5-bit, etc
* Você vê um número como -16k? Esse é um tamanho de contexto aumentado (ou seja, quanto tempo sua conversa pode ficar antes do modelo esquecer o início do seu chat)! Note que tamanhos de contexto maiores exigem mais VRAM.

## Instalando um servidor LLM: Oobabooga ou KoboldAI

Com o LLM agora no seu PC, precisamos baixar uma ferramenta que atuará como intermediária entre o SillyTavern e o modelo: ela carregará o modelo e exporá sua funcionalidade como uma API web HTTP local com a qual o SillyTavern pode conversar, da mesma forma que o SillyTavern conversa com serviços web pagos como OpenAI GPT ou Claude. A ferramenta que você usa deve ser KoboldAI ou Oobabooga (ou outras ferramentas compatíveis).

Este guia cobre ambas as opções, você só precisa de uma.

!!!warning
Se você estiver hospedando o SillyTavern no Docker, use **http://host.docker.internal:\<port\>** em vez de **http://127.0.0.1:\<port\>**. Isso ocorre porque o SillyTavern se conecta ao endpoint da API do servidor rodando no contêiner Docker. A pilha de rede do Docker é separada da do host e, portanto, as interfaces de loopback não são compartilhadas.
!!!

### Baixando e usando KoboldCpp (Nenhuma instalação necessária, modelos GGUF)

1. Visite https://koboldai.org/cpp onde você verá a versão mais recente com vários arquivos que você pode baixar.
No momento da escrita, a versão CUDA mais recente que eles listam é cu12, que funcionará melhor em GPUs NVIDIA modernas. Se você tem uma GPU mais antiga ou de uma marca diferente, pode usar o koboldcpp.exe regular. Se você tem uma CPU antiga, é possível que o KoboldCpp trave quando você tentar carregar modelos. Nesse caso, tente a versão _oldcpu para ver se isso resolve seu problema.
2. O KoboldCpp não precisa ser instalado. Assim que você iniciar o KoboldCpp, você imediatamente poderá selecionar seu modelo GGUF, como o vinculado acima, usando o botão Browse ao lado do campo Model.
3. Por padrão, o KoboldCpp funciona com um máximo de 4K de contexto, mesmo se você definir isso mais alto no SillyTavern. Se você deseja executar um modelo com contexto maior, certifique-se de ajustar o controle deslizante de contexto nesta tela antes de iniciar o modelo. Tenha em mente que mais tamanho de contexto significa requisitos de memória (de vídeo) maiores. Se você definir isso muito alto ou carregar um modelo muito grande para o seu sistema, o KoboldCpp começará automaticamente a usar sua CPU para as camadas que não cabem na sua GPU, o que será muito mais lento.
4. Clique em Launch. Se tudo correr bem, uma nova página da web será aberta com o KoboldAI Lite, onde você pode testar se tudo funciona corretamente.
5. Abra o SillyTavern e clique em API Connections (2º botão na barra superior)
6. Defina API como Text Completion e o API Type como KoboldCpp.
7. Defina a URL do servidor como <http://127.0.0.1:5001/> ou o link que o KoboldCpp lhe deu caso não esteja rodando no mesmo sistema (Você pode ativar o modo Remote Tunnel do KoboldCpp para obter um link que pode ser acessado de qualquer lugar).
8. Clique em Connect. Deve se conectar com sucesso e detectar kunoichi-dpo-v2-7b.Q6_K.gguf como o modelo.
9. Converse com um personagem para testar se funciona.

### Dicas para otimizar a velocidade do KoboldCpp
1. Flash Attention ajudará a reduzir os requisitos de memória. Pode ser mais rápido ou mais lento dependendo do seu sistema e permitirá que você encaixe mais camadas na sua GPU do que o padrão.
2. O KoboldCpp deixará algum espaço para outro software quando adivinhar camadas para evitar problemas. Se você tem poucos programas abertos e não consegue encaixar o modelo inteiramente na GPU, você pode conseguir adicionar algumas camadas extras.
3. Se o modelo usa muita memória para o tamanho do contexto, você pode diminuir isso Quantizando o KV. Isso reduzirá a qualidade da saída, mas pode ajudá-lo a colocar mais camadas na GPU. Para fazer isso, vá para a aba Tokens no KoboldCpp e desative Context Shifting e ative Flash Attention. Isso desbloqueará o controle deslizante Quantized KV Cache. Um número menor significa menos memória / inteligência do modelo.
4. Executando o KoboldCpp em um sistema mais lento onde leva muito tempo para processar o prompt? Context Shifting funciona melhor quando você evita usar Lorebooks, randomização ou outros recursos que mudam dinamicamente a entrada. Deixar o context shifting ativado ajudará você a evitar longos tempos de reprocessamento.

### Instalando Oobabooga

Aqui está um procedimento de instalação mais correto/à prova de falhas:

1. git clone <https://github.com/oobabooga/text-generation-webui> (ou baixe o repositório como um .zip no seu navegador e extraia)
2. Execute start_windows.bat ou o que for apropriado para seu sistema operacional
3. Quando solicitado, selecione seu tipo de GPU. Mesmo se você pretende usar GGUF/CPU, se sua GPU estiver na lista, selecione-a agora, porque isso lhe dará a opção de usar uma otimização de velocidade mais tarde chamada GPU sharding (sem ter que reinstalar do zero). Se você não tem uma dGPU de nível para jogos (NVIDIA, AMD), selecione None.
4. Aguarde a conclusão da instalação
5. Coloque kunoichi-dpo-v2-7b.Q6_K.gguf em text-generation-webui/models
6. Abra text-generation-webui/CMD_FLAGS.txt, delete tudo dentro e escreva: --api
7. Reinicie o Oobabooga
8. Visite <http://127.0.0.1:5000/docs>. Ele carrega uma página FastAPI? Se não, você errou em algum lugar.

### Carregando nosso modelo no Oobabooga

1. Abra <http://127.0.0.1:7860/> no seu navegador
2. Clique na aba Model
3. No menu suspenso, selecione nosso modelo Kunoichi DPO v2. Ele deve ter selecionado automaticamente o carregador llama.cpp.
4. (Opcional) Mencionamos 'GPU offload' várias vezes anteriormente: essa é a configuração n-gpu-layers nesta página. Se você quiser usá-la, defina um valor antes de carregar o modelo. Como referência básica, defini-la como 30 usa pouco menos de 6GB de VRAM para modelos 13B e menores. (varia com a arquitetura e tamanho do modelo)
5. Clique em Load

### Configurando o SillyTavern para conversar com o Oobabooga

1. Clique em API Connections (2º botão na barra superior)
2. Defina API como Text Completion
3. Defina API Type como Default (Oobabooga)
4. Defina a URL do servidor como <http://127.0.0.1:5000/>
5. Clique em Connect. Deve se conectar com sucesso e detectar kunoichi-dpo-v2-7b.Q6_K.gguf como o modelo.
6. Converse com um personagem para testar se funciona

## Conclusão

Parabéns, agora você deve ter um LLM local funcionando.
