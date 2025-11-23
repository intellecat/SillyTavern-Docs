---
order: 60
route: /usage/prompts/cfg/
---

# CFG

Página escrita por: kingbri

Contribuidores: kingbri, Guillaume "Vermeille" Sanchez, AliCat

## O que é?

CFG, ou classifier-free guidance, é um método usado para ajudar a tornar partes de um prompt menos ou mais proeminentes.

### APIs de Backend Suportadas

Atualmente, os backends suportados são oobabooga's textgen WebUI, NovelAI e TabbyAPI.
A NovelAI tinha sua própria [documentação para CFG](https://web.archive.org/web/20240917150051/https://docs.novelai.net/text/cfg.html).

AVISO: CFG aumenta o uso de VRAM devido à ingestão de mais de 1 prompt! Se a memória da sua GPU acabar ao gerar um prompt com CFG ligado, considere reduzir o tamanho do seu contexto, usar um modelo de parâmetros menores ou desligar o CFG completamente.

---

## Configuração

Acessar as configurações de CFG é o mesmo que acessar a Author's note:

![CFGhamburgermenupng](/static/cfg-hamburger.png)

E aqui está como o painel de CFG se parece:

![CFGchatpanelpng](/static/cfg-panel.png)

Existem quatro dropdowns no painel de CFG:

- Chat CFG

  - Escopa a escala e prompts de CFG apenas para este chat
- Character CFG

  - Escopa a escala e prompts de CFG para o personagem especificado
- Global CFG

  - Substitui globalmente a escala e prompts de CFG (também substitui o preset do modelo!)
- CFG Advanced Settings (anteriormente chamado CFG Prompt Cascading)

  - Um lugar para combinar prompts dos 3 dropdowns anteriores e definir profundidade de inserção.

NOTA: Se a escala de orientação estiver definida como 1, nada será enviado, pois é quando o CFG está em um estado "desligado".

#### Chats em Grupo

Em chats em grupo, o painel de escala de CFG se parece com isto:

![CFGpanelgcpng](/static/cfg-groups.png)

A principal mudança é que o CFG de personagem é removido e uma caixa de seleção chamada `Use Character CFG Scales` está presente no dropdown de CFG de chat. Isso permite que a escala de orientação do personagem atual seja usada em vez do que a escala de CFG de chat está definida.

A principal utilidade deste recurso é alterar a escala com base nas necessidades individuais de cada personagem.

Além disso, marcar a caixa `Character Negatives` em prompt cascading anexará os prompts negativos independentes do personagem junto com os do chat (se habilitados).

---

## Conceitos

### Isso não está no Stable Diffusion?

Sim e não. CFG com LLMs funciona de uma maneira diferente do que se pode estar acostumado no Stable Diffusion. CFG baseado em LLM funciona no princípio de "mistura de prompts". A fórmula de CFG pega um prompt positivo e negativo, então mistura as *diferenças* entre eles. A partir daí, um prompt combinado é enviado e uma resposta é gerada!

Aqui está uma ilustração para ajudar a visualizar este conceito. O vermelho representa o prompt negativo, o azul representa o prompt neutro, e o roxo representa o resultado misto que é interpretado. Todo o espaço em branco é o mesmo em todos os 3 prompts, então eles não são usados para mistura de CFG.

![stcfgdiagrampng](/static/cfg-diagram.png)

Se você quiser saber mais sobre CFG e LLMs, o artigo original do Vermifuge está localizado aqui. Eu sugiro dar uma lida/escutada:

- Artigo - [[2306.17806] Stay on topic with Classifier-Free Guidance (arxiv.org)](https://arxiv.org/abs//2306.17806)

- Versão em áudio - [https://www.youtube.com/watch?v=MGY00YFcyco](https://www.youtube.com/watch?v=MGY00YFcyco)


### Eu preciso de prompts de CFG?

Não! Prompts de CFG são completamente opcionais. Apenas ajustar a escala de orientação acima de `1` também ajudará a produzir um efeito nas respostas, o que pode acentuar chats e interação de personagens.

### O que faz um bom prompt de CFG?

Então, estabelecemos que o prompting de CFG não é o mesmo que tags negativas e embeddings do Stable Diffusion. Como fazemos um prompt?

Aviso: Isso assume que você criou um personagem usando PLists e Ali:Chat. Se você não criou, sinta-se à vontade para experimentar várias técnicas de prompting.

Digamos que eu tenha um personagem chamado "John". John deveria se sentir feliz e animado o tempo todo a partir de seus diálogos de exemplo. No entanto, ao conversar com John, ele às vezes fica triste e deprimido.

Para remover isso, CFG vem ao resgate! Basta fazer o prompt negativo `[John's feelings: sad, depressed]` para ajudar a remover as partes de tristeza. Você pode opcionalmente fazer o prompt positivo `[John's feelings: happy, joyful]` para destacar ainda mais as partes felizes de John.

### Prompts Positivos

Eu abordei isso na seção anterior, mas gostaria de tocar nisso um pouco mais. Prompts positivos são usados para acentuar ainda mais partes de um personagem. Vamos usar John novamente como nosso exemplo. Ao torná-lo mais feliz com um prompt positivo de `[John's feelings: happy, joyful]`, John deveria começar a produzir diálogo com um sentimento mais feliz do que se o prompt positivo não fosse incluído.

### Mas...

Estas são apenas **diretrizes soltas** da experiência com um formato de personagem específico. Existem muitas outras maneiras de criar prompts que você deve experimentar. Sinta-se à vontade para compartilhar seus pensamentos com outros usuários!

### Escala de Orientação

Aqui está uma regra geral. Uma escala de orientação de `1` significa que o CFG está desabilitado. Na verdade, o SillyTavern não enviará nada para seu backend se a escala de orientação for 1. Uma escala de orientação `>1` dará os resultados mostrados nas outras seções em graus variados.

No entanto, uma escala de orientação de `<1` dará o efeito *oposto*, já que o prompt negativo é usado como prompt principal aqui.

Vamos usar o exemplo com John novamente. O prompt negativo é `[John's feelings: sad, depressed]` e o prompt positivo é `[John's feelings: happy, joyful]` com uma escala de orientação de `0.8`.

Isso, por sua vez, acentuará o prompt *negativo* mais e você verá John começar a agir mais triste do que o normal, em vez de mais feliz.

tldr; Use uma escala de orientação de `1.5` e trabalhe para cima e para baixo a partir daí com base em suas saídas.

### Prompt Cascading

Negativos e positivos podem ser cascateados entre tipos de CFG (os tipos sendo por chat, por personagem e substituições globais). Veja o cabeçalho de Configuração para mais informações.

### Profundidade de Inserção

Siga a regra básica: Quanto mais baixo algo está localizado no prompt, mais influente é para a resposta. Para conversar, eu recomendo usar a profundidade padrão de `1`, pois é muito flexível com outros componentes do SillyTavern.

No entanto, se você quiser experimentar, uma profundidade de inserção de `0` está disponível. No entanto, isso pode alterar drasticamente como sua resposta ficará e NÃO é recomendado usar prompt cascading aqui!
