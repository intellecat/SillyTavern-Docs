---
route: /extensions/smart-context/
---

# Smart Context

## **ESTA EXTENSÃO NÃO É MAIS MANTIDA E NÃO É RECOMENDADA PARA USO. CONSIDERE [CHAT VECTORIZATION](/extensions/Chat-vectorization.md) COMO UMA POSSÍVEL ALTERNATIVA.**

!!!warning Aviso
O uso desta extensão não garante uma melhor experiência de chat ou memória aprimorada de qualquer tipo. Use apenas se você entender todas as implicações da utilização de banco de dados vetorial.
!!!

### O que é isso?

Smart Context é uma extensão SillyTavern que usa a [biblioteca ChromaDB](https://www.trychroma.com) para dar aos seus personagens de IA acesso a informações que existem fora do limite normal do contexto do histórico do chat.

### Como isso é útil?

Se você tiver um chat muito longo, a maioria do conteúdo está fora da janela de contexto usual e, portanto, indisponível para a IA ao escrever uma resposta.

Smart Context automaticamente pega todo o histórico do arquivo de chat e o coloca em um banco de dados vetorial. Este banco de dados é então pesquisado cada vez que você insere algo novo no chat, e se mensagens com palavras-chave correspondentes forem encontradas, essas mensagens de chat são colocadas no contexto para que a IA possa vê-las ao escrever sua próxima resposta.

***

### Instruções de Configuração

1. Atualize SillyTavern para pelo menos a versão 1.10.6.
2. Instale a extensão "Smart Context" no menu "Download Extensions & Assets" no painel Extensions (ícone de blocos empilhados).
3. Instale ou Atualize [Extras](https://github.com/SillyTavern/SillyTavern-extras) para a versão mais recente. Alternativamente, use o [notebook Colab](https://colab.research.google.com/github/SillyTavern/SillyTavern/blob/release/colab/GPU.ipynb).
4. *Instalações locais apenas:* Instale requirements-complete.txt para Extras (mesmo se você fez isso uma vez antes em uma instalação anterior).
5. Execute Extras com o módulo chromadb habilitado: `python server.py --enable-modules=chromadb`

#### Recebendo um erro ao instalar ChromaDB?

```
ERROR: Could not build wheels for hnswlib, which is required to install pyproject.toml-based projects
```

Instalar o pacote chromadb requer uma das seguintes opções:

- Ter ferramentas de build Visual C++ instaladas: <https://visualstudio.microsoft.com/visual-cpp-build-tools/>
- Instalar hnswlib do conda: `conda install -c conda-forge hnswlib`

***

### Configuração

Uma vez que Smart Context esteja habilitado, você deve configurá-lo na UI do SillyTavern.
A configuração de Smart Context pode ser feita dentro do menu Extensions ![STExtensionMenuIcon](/static/extensions/menu-icon.png)

![Smart Context Config Panel](/static/extensions/smart-context.png)

Existem 4 conceitos principais a serem conhecidos:

- Chat History Preservation
- Memory Injection Amount
- Individual Memory Length
- Injection Strategy

***

#### SmartContext só inicia após 10 mensagens estarem no histórico do chat

- No início de um novo chat, ChromaDB está inativo.
- Uma vez que o chat tenha acumulado 10 mensagens, ele começará a gravar todas as mensagens no banco de dados e recuperar mensagens conforme necessário.

#### Chat History Preservation ('kept mesages')

Por padrão, ChromaDB manterá tantas mensagens de histórico de chat natural recentes quanto especificado no controle deslizante.
Quaisquer mensagens além desse valor serão removidas do seu prompt enviado, e se 'memórias' existirem no banco de dados, elas serão adicionadas no lugar das mensagens de histórico de chat mais antigas (veja Estratégia abaixo).

***

#### Memory Injection Amount

O número máximo de 'memórias' que Smart Context inserirá no contexto.
Nem toda tentativa de injeção obterá esse valor total.
Se você enviar uma entrada relacionada a 'dogs' e apenas uma outra mensagem no DB estiver relacionada a dogs, então apenas 1 item será inserido.

***

#### Individual Memory Length

Este é o comprimento máximo permitido para cada 'memória' injetada.
Isso está em **CARACTERES** (não tokens).
Se definido muito pequeno, a memória pode ser cortada no meio.

Exemplo:

`Ross: I like dogs with long fur and fluffy tails. I dislike dogs with short fur and short tails.`

Esta 'memória' de banco de dados tem 103 caracteres de comprimento, então você precisaria definir o controle deslizante para pelo menos `103` para puxá-la inteiramente para o contexto.

Se o controle deslizante for menor que 103, a mensagem seria cortada e injetada assim.

***

### Injection Strategy

#### Replace oldest history

Esta estratégia mantém X mensagens recentes, remove todas as mensagens antes disso e as substitui por 'memórias'.

Vantagem

- menos provável de estourar seu limite de contexto
- memórias existentes perto do topo do contexto terão menos impacto imediato na resposta enquanto ainda fornecem 'informações de fundo'.

Desvantagem

- mensagens antigas são inseridas diretamente no histórico do chat sem demarcação especial, e geralmente não têm relevância natural imediata para as mensagens de histórico de chat natural preservadas. Isso pode confundir modelos de IA menos inteligentes.

#### Add to Bottom

Esta estratégia deixa o histórico do chat em seu estado natural e adiciona 'memórias' **depois** dele dentro de um cabeçalho de colchete formatado.
Isso significa que o controle deslizante 'kept messages' está efetivamente desabilitado.

Vantagem

- não encurta ou altera o histórico de chat natural atual
- 'memórias' existem após o chat e têm um impacto mais forte na próxima resposta da IA

Desvantagem

- como nenhum item de chat está sendo removido/substituído, há uma chance maior de você estourar seu limite de contexto.
- como as memórias existem muito perto do final do prompt, elas podem ter MUITO efeito na resposta da IA.

#### Custom Depth

Esta estratégia deixa o histórico do chat em seu estado natural e adiciona 'memórias' na profundidade que você determinar dentro do template que você especificar.
Isso significa que o controle deslizante 'kept messages' está efetivamente desabilitado.
A mensagem de injeção personalizada deve incluir a palavra de template `{{memories}}` que é onde todas as memórias consultadas serão colocadas.

Vantagem

- flexibilidade para experimentar com o posicionamento de memória
- introduções personalizáveis à memória dentro do contexto

Desvantagem

- como nenhum item de chat está sendo removido/substituído, há uma chance maior de você estourar seu limite de contexto.


#### Use % Strategy

Nota: Isso não é compatível com a estratégia 'Add to Bottom', que não remove nenhuma mensagem.

Ao usar a estratégia 'Replace Oldest History', marcar esta caixa habilitará o controle deslizante para selecionar uma porcentagem do histórico de chat em contexto para substituir por memórias SmartContext. Também desabilitará os dois controles deslizantes para selecionar manualmente o número de mensagens.

Esta estratégia calcula automaticamente uma porcentagem do histórico do chat a ser substituída por memórias SmartContext, em vez de um número fixo de mensagens.

Vantagem

- mais fácil do que calcular manualmente o número de mensagens você mesmo
- ajusta com o tamanho de contexto disponível, aplicando a mesma porcentagem a espaços de prompt pequenos e grandes

Desvantagem

- cálculos de quanto histórico remover podem ser ligeiramente imprecisos, pois são baseados em tokens estimados por mensagem
- arredonda o número de mensagens a remover para o número mais próximo divisível por 5 (0, 5, 10, 15, 20, etc), então não é tão fino quanto a seleção numérica manual.

***

### Memory Recall Strategy

#### Recall only from this chat

Este é o comportamento padrão do smart-context e puxa 'memórias' apenas da coleção ChromaDB para este chat específico.

#### Recall from all character chats

Este é um comportamento experimental do smart-context que puxa 'memórias' de todas as coleções ChromaDB para o personagem selecionado.
Hipoteticamente isso deveria permitir o desenvolvimento de um conjunto de memória mais robusto abrangendo muitas interações.
Recomendado que isso seja usado com estratégias 'Add to Bottom' ou 'Custom Depth' e 'kept messages' definido para um número baixo para que ChromaDB puxe da memória mais cedo.

### Usando Smart Context

Uma vez habilitado e configurado, Smart Context acontece automaticamente.

ChromaDB cria um novo banco de dados para cada chat que é aberto dentro do SillyTavern.
Este banco de dados é automaticamente preenchido com todo o histórico do chat.

Você também pode inserir manualmente arquivos de texto no banco de dados.

Esses arquivos de texto não precisam ser chats. Eles podem ser qualquer coisa (entradas da wikipedia, fanfic, etc).

#### Purgando o Banco de Dados

Você pode usar o botão 'Purge DB' para limpar o banco de dados para o chat atual.

Isso pode ser útil se você encontrar memórias imprecisas que foram armazenadas (como mensagem de chat que você deletou ou editou desde então).

***

### FAQ

#### O que acontece com os bancos de dados quando termino de conversar? Posso salvá-los?

Para servidores Extras instalados localmente, Smart Context salva os bancos de dados. Não há necessidade de salvá-los manualmente em casos de uso usuais.

Para usuários colab, os bancos de dados são apagados quando o servidor extras desliga. Use o botão de exportação para salvar o banco de dados como um arquivo JSON e importá-lo na próxima vez que quiser usá-lo.

**Geralmente não há necessidade de salvar bancos de dados Smart Context.**

Atualmente temos um recurso de Importação/Exportação, que permite salvar o DB do chat e usá-lo novamente em uma data posterior.

#### Posso fazer um banco de dados grande para todos os meus chats referenciarem?

Isso não seria um bom uso das capacidades do Smart Context.
Recomendamos usar World Info para este propósito.
