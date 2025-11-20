---
route: /extensions/regex/
---

# Regex

## O que é isso?

A extensão Regex permite que o usuário detecte automaticamente padrões específicos em uma string de texto (chamados 'sequências') e aplique manipulações (substituições) a eles. Pode ser uma ferramenta poderosa quando usada em conjunto com outros recursos do SillyTavern como [Quick Replies ou STscript](/For_Contributors/st-script.md), ou simplesmente uma maneira de remover certas palavras de um chat.

## Links Úteis

**Este documento não explicará o processo de escrever uma sequência RegEx em profundidade. Existem muitos recursos online para ajudá-lo com isso.**

- [https://regexr.com](https://regexr.com)

- [https://regex101.com](https://regex101.com)

- [https://extendsclass.com/regex-tester.html](https://extendsclass.com/regex-tester.html)

- [https://en.wikipedia.org/wiki/Regular_expression](https://en.wikipedia.org/wiki/Regular_expression)

## Pré-requisitos

Regex é uma extensão integrada do SillyTavern, então nenhuma configuração adicional é necessária.

Você pode encontrar suas configurações no painel **<i class="fa-solid fa-cubes"></i> Extensions**.

## Casos de Uso Comuns

RegEx é frequentemente usado para aplicar uma função localizar-substituir em certas palavras no chat, para adicionar estilos markdown a certas palavras ou tipos de frases, ou para retornar um valor booleano a um STscript.

## Lista de Scripts

![RegEx Extension Script List](/static/extensions/regex-listview.png)

- Os botões no topo são usados para criar um novo script.
  - Scripts 'Global' serão aplicados a todos os personagens e serão salvos em `settings.json`.
  - Scripts 'Scoped' serão aplicados apenas ao personagem atualmente ativo e serão salvos nos dados do card do personagem.
- 'Import' permite importar scripts RegEx que foram exportados de outra instância do SillyTavern.

Abaixo disso está uma lista de seus scripts com alguns botões de ação.

- Alças de arrastar (três barras horizontais à esquerda do nome do script) permitem arrastar/soltar os scripts em qualquer ordem que desejar.
- O interruptor principal on/off pode ser alternado rapidamente para habilitar ou desabilitar o script sem alterar mais nada. Scripts desabilitados são mostrados com estilo ~~tachado~~. **Se um script estiver desabilitado aqui, ele será inativável por um Quick Reply ou STscript.**
- O botão 'Edit' (lápis) abrirá o editor de script RegEx.
- 'Move to scoped' (seta para baixo) converterá um script global em um script scoped e o aplicará ao personagem atual. No sentido inverso (seta para cima), converteria um script scoped em global.
- 'Export' fará com que seu navegador baixe um arquivo `.json` exportado do Script, que pode então ser compartilhado e importado em outra instância do SillyTavern.
- 'Delete' (lixeira) exclui o script.

## Editor RegEx

![RegEx Editor](/static/extensions/regex-editor.png)

- **Test Mode**: Isso abrirá uma visualização de comparação no topo do editor. Digite algum texto na caixa 'Input', e os resultados do seu script RegEx serão mostrados na caixa Output. É uma ferramenta de debug valiosa, pois atualizará a caixa Output em tempo real conforme você faz alterações nas configurações do script.

- **Name**: O rótulo para o script mostrado na lista de scripts da extensão. **Isso também é usado para direcionar o script ao acioná-lo via comando slash ou STscript.**

- **Find Regex**: Esta é a Expressão Regular usada para detectar seu padrão de texto alvo. Esta é geralmente a parte mais complexa de qualquer script RegEx, e é o lugar mais fácil de cometer erros. Consulte os links no topo da página para informações sobre como escrever uma sequência RegEx. Esta caixa pode resolver os valores de [macros comuns do SillyTavern](/Usage/Characters/macros.md) (como \{\{user\}\}, \{\{char\}\}, etc) se 'Macros in Find Regex' estiver configurado para fazer isso (veja abaixo).

- **Replace With**: Isto é o que substituirá a sequência correspondida. Em um exemplo muito simples, se seu 'Find Regex' for `apple`, e seu 'Replace With' for `orange`, a primeira ocorrência de 'apple' seria automaticamente alterada para 'orange' em qualquer texto onde o script for aplicado.

  - Adicionar a macro específica da extensão \{\{match\}\} nesta caixa inserirá a sequência de texto totalmente correspondida. Isso é comumente usado para aplicar estilos a palavras específicas. Voltando ao exemplo acima, se \*\*\{\{match\}\}\*\* fosse colocado na caixa 'Replace With', todas as ocorrências da palavra 'apple' seriam substituídas por `**apple**`, o que aplicaria o estilo markdown negrito a ela.

  - Variáveis como $1, $2, $3 etc podem ser usadas para inserir o que são chamados 'Capture Groups'. Estas são substrings localizadas na sequência de texto correspondida pela sequência 'Find Regex'. **Observe que usar essas variáveis requer que a expressão correspondente contenha conjuntos de parênteses para definir qual parte da string correspondida conta como um grupo capturado.** Consulte os links no topo para referência sobre como configurar Capture Groups.

- **Trim Out**: texto colocado nesta caixa será removido da sequência de texto correspondida antes que o processo 'Replace With' seja aplicado. Por exemplo, se nossa correspondência fosse 'apple', e a caixa Trim Out contém 'le', então as letras 'le' seriam removidas primeiro antes que o processo 'Replace With' seja aplicado. Como nossa caixa 'Replace With' contém \*\*\{\{match\}\}\*\*, resultaria em `**app**` sendo colocado como substituição para 'apple' (primeiro 'le' é removido, e o texto correspondido restante recebe o estilo markdown negrito). Múltiplos trims podem ser aplicados adicionando uma nova linha entre cada string que você deseja remover.

- **Affects**: Esta lista de caixas de seleção define as fontes de texto às quais o script RegEx será aplicado.
  - 'User Input': script será executado contra o conteúdo da entrada digitada do usuário depois que ele apertar Send.
  - 'AI Response': script será executado contra o conteúdo da resposta da IA depois de ser recebida.
  - 'Slash Commands': script será executado contra os valores inseridos no prompt/chat por comandos slash.
  - 'World Info': script será executado contra o conteúdo de entradas de World Info conforme são injetadas no prompt. **Requer 'Alter Outgoing Prompt' estar marcado (ou ambas as caixas de efemeridade estarem desmarcadas).**
  - 'Reasoning': script será executado contra o conteúdo do objeto 'reasoning' retornado por APIs de Chat Completion como Gemini ou Deepseek. Se 'Alter Outgoing Prompt' estiver marcado em Ephemerality, o script também será aplicado a quaisquer blocos de reasoning que forem adicionados ao prompt em turnos de chat subsequentes.
  - **Se tudo aqui estiver desmarcado, o script nunca será ativado durante o chat normal, mas ainda pode ser ativado via comando slash ou STscript.**

- **Other Options**:
  - 'Disabled' impede que o script seja executado. Isso é usado como uma substituição para evitar que o script seja executado quando você simplesmente não quer alterar nenhuma das configurações do script e/ou não quer desabilitá-lo inteiramente via o interruptor na lista de scripts (pois fazer isso impediria comandos slash de acioná-lo).
  - 'Run on Edit' faz o script também ser executado após uma mensagem de chat ser editada. Se isso estiver desmarcado, o conteúdo das mensagens de chat editadas não acionará o script.

- **Macros in Find Regex**: Selecione se deseja ou não substituir macros (como \{\{user\}\}, \{\{char\}\}, etc) que estão presentes na sequência da caixa Find Regex.
  - 'Don't Substitute' fará com que quaisquer macros do SillyTavern sejam ignoradas para que o script RegEx as trate literalmente ao pesquisar.
  - 'Raw' enviará o valor da macro literalmente. Isso pode alterar a maneira como seu script RegEx pesquisa o texto se o valor da macro contiver certos caracteres especiais.
  - 'Escaped' adicionará uma barra de escape RegEx `\` antes de cada caractere para garantir que eles não alterem acidentalmente a sequência RegEx geral. Isso pode ser útil se você tiver certos caracteres especiais nos valores da macro.

### Configurações de Profundidade

As configurações de Min/Max Depth fornecem controle preciso sobre quais mensagens no histórico do chat seu padrão regex afetará:

- **Min Depth**: Afeta apenas mensagens que estão pelo menos N níveis de profundidade no histórico do chat
  - 0 = última mensagem
  - 1 = penúltima mensagem
  - etc.
  - Quando em branco (definido como 'Unlimited'), ou -1, também afetará a mensagem a continuar na ação Continue

- **Max Depth**: Afeta apenas mensagens não mais profundas que N níveis no histórico do chat
  - Deve ser maior que Min Depth para que o regex se aplique
  - Prompts de sistema e prompts utilitários não são afetados por essas configurações

Por exemplo, definir Min Depth como 0 e Max Depth como 2 aplicaria seu regex apenas às três mensagens mais recentes no chat.

### Flags

Por padrão, o padrão Find Regex é sensível a maiúsculas/minúsculas e se aplica apenas à primeira correspondência. Para ajustar esse comportamento, bem como outras flags RegEx, você pode adicioná-las assim:

```txt
/yourpattern/flags
```

Exemplo: `/yourpattern/gi` corresponderá a todas as instâncias de 'yourpattern' no texto, independentemente de maiúsculas/minúsculas.

Algumas das flags mais comuns são:

- `i` : insensível a maiúsculas/minúsculas
- `g` : global (aplica a todas as correspondências, não apenas à primeira)
- `s` : dotAll (trata a entrada como uma única linha, então `.` corresponderá a novas linhas)
- `m` : multi-linha (trata a entrada como múltiplas linhas, então `^` e `$` correspondem ao início/fim de cada linha, não apenas a string inteira)
- `u` : unicode (trata a entrada como unicode, então `\d`, `\w`, etc. corresponderão a caracteres unicode)

Para mais informações sobre flags RegEx, consulte a seguinte página MDN: [Advanced searching with flags](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_expressions#advanced_searching_with_flags)

### Efemeridade

Por padrão (quando nenhuma caixa aqui está marcada), um script RegEx editará diretamente os valores de texto armazenados dentro do arquivo JSONL do chat. Isso garante que tanto o prompt de saída quanto a exibição do chat sempre contenham os mesmos valores. No entanto, essas alterações no arquivo de chat são irreversíveis.

Se você não quiser que isso aconteça, pode habilitar qualquer uma das caixas de seleção aqui para limitar os efeitos do script RegEx apenas à exibição ou ao prompt de saída.

Se apenas uma das caixas estiver marcada, não haverá alterações feitas no arquivo de chat, mas **apenas o item marcado** será alterado. Isso significa que você verá uma coisa, mas o LLM verá outra. Use isso com cuidado.

Se ambas estiverem selecionadas, o script funcionará normalmente em todos os aspectos EXCETO que não gravará nenhuma alteração no arquivo de chat.

## Uso Avançado

Embora RegEx seja comumente usado como uma ferramenta simples de Localizar/Substituir, também pode ser usado de maneiras mais complexas.

Por exemplo, a caixa 'Replace With' poderia incluir um conjunto de regras CSS e HTML para adicionar um elemento HTML estilizado específico ao seu chat sempre que uma certa palavra for encontrada. Isso exigirá que a caixa `Show <tags> in responses` esteja desmarcada no painel User Settings.

O script também pode ser configurado para nunca acionar durante o uso normal, mas poderia ser acionado via comando slash como parte de uma verificação de lógica dentro de um STscript. A caixa 'Replace With' incluiria um valor único que o script reconhece para indicar se uma verificação de lógica é verdadeira ou falsa. Isso expande a utilidade do RegEx para as capacidades completas de todos os comandos slash, permitindo níveis verdadeiramente ilimitados de controle e automação com base no conteúdo do chat.
