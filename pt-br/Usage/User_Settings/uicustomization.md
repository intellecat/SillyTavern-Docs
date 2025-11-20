---
order: 20
route: /usage/core-concepts/uicustomization/
---

# UI Customization

## UI Theme

### Gerenciamento de Temas

Arquivos de tema permitem que você salve, compartilhe e reutilize suas personalizações de UI. Você pode manter múltiplos temas para diferentes estados de espírito ou propósitos, e alternar entre eles instantaneamente.

* Importar/Exportar arquivos de tema
* Excluir temas existentes
* Salvar alterações no tema atual
* Salvar como novo tema

Todas as configurações nesta seção são salvas no tema atual. Se você trocar de tema, as configurações serão substituídas pelas configurações do novo tema.

### Display Settings

Essas opções de exibição afetam como os personagens e mensagens são apresentados na interface do chat.

#### Avatar Style

Escolha entre Circle, Square, Rectangle ou Rounded Square. Esta configuração se aplica tanto aos avatares de usuário quanto de IA.

#### Chat Style

| Style        | Description                                                                                                                                                    | [Slash command](/For_Contributors/st-script.md#ui-styling) |
|--------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------|
| **Flat**     | Estilo limpo e contínuo de "log de chat", uma tela plana para suas interações de IA ganharem vida.                                                                 | `/flat`<br>`/default`                                      |
| **Bubbles**  | Estilo de "mensageiro instantâneo" com bolhas distintas para cada mensagem, cantos arredondados deliciosos e um efeito 3D sutil.                                          | `/bubble`<br>`/bubbles`                                    |
| **Document** | Aparência compacta semelhante a documento com layout focado em texto. Oculta avatares, carimbos de data/hora e botões de controle de mensagem para mensagens passadas. | `/single`<br>`/story`                                      |

### Notifications

Defina uma posição onde os popups de notificação (mensagens toast) aparecerão na tela.

* Top Left
* Top Center (padrão)
* Top Right
* Bottom Left
* Bottom Center
* Bottom Right

### Theme Colors

Personalize o esquema de cores de cada elemento da UI para criar seu tema perfeito. As cores podem ser selecionadas usando um seletor de cores e incluem opções de transparência quando aplicável.

* Main Text
* Italics Text
* Underlined Text
* Quote Text
* Text Shadow
* Chat Background
* UI Background
* UI Border
* User Message
* AI Message

### Layout & Visual Settings

Ajuste fino da apresentação visual da interface com esses controles deslizantes.

* **Chat Width**: Ajuste a largura da janela de chat (25-100% da tela)
* **Font Scale**: Personalize o tamanho do texto (0.5-1.5x)
* **Blur Strength**: Controle o desfoque do painel da UI (0-30)
* **Shadow Width**: Ajuste a intensidade da sombra do texto (0-5)

### Theme Toggles

Esses interruptores controlam vários recursos e comportamentos da UI. Algumas opções podem melhorar o desempenho em dispositivos de baixo desempenho, enquanto outras adicionam informações úteis ou funcionalidade à interface de chat.

* **Reduced Motion**: Desabilita animações e transições
* **No Blur Effect**: Remove desfoque de fundo para melhor desempenho
* **No Text Shadows**: Desabilita efeitos de sombra de texto
* **[Visual Novel mode](Visual-Novel.md)**: Chat compacto com sprite de fundo
* **Expand Message Actions**: Sempre mostra menu de contexto completo da mensagem
* **Zen Sliders**: Controles de parâmetros simplificados
* **Mad Lab Mode**: Intervalos de parâmetros sem restrições
* **Message Timer**: Mostra tempo de geração de resposta da IA
* **Chat Timestamps**: Exibe carimbos de data/hora das mensagens
* **Model Icons**: Mostra ícones de modelo de IA para mensagens
* **Message IDs**: Exibe números sequenciais de mensagens
* **Hide Chat Avatars**: Remove avatares do chat
* **Message Token Count**: Mostra contagens de tokens por mensagem
* **Compact Input Area**: Entrada de linha única (Apenas Mobile)
* **Swipe # for All Messages**: Mostra números de swipe em todas as mensagens
* **Characters Hotswap**: Botões de seleção rápida para personagens favoritos
* **Avatar Hover Magnification**: Efeito de zoom ao passar o mouse sobre avatar
* **Tags as Folders**: Organiza personagens usando tags como pastas
* **Click to Edit**: Clique nas mensagens para abrir rapidamente um editor de mensagens

### Custom CSS

Permite que você aplique estilos CSS personalizados para personalizar ainda mais a aparência da interface de chat.

Use <i class="fa-fw fa-solid fa-maximize" title="Expand icon"></i> **Expand** para expandir a janela do editor para melhor visibilidade e edição.

Se você trocar de tema, seu CSS personalizado será substituído pelo CSS personalizado do novo tema. Certifique-se de salvar seu CSS personalizado em um tema se quiser mantê-lo ao trocar de temas.

Se você usar muito CSS personalizado, ou quiser usar o mesmo CSS personalizado com vários temas, a [extensão CSS Snippets](https://github.com/LenAnderson/SillyTavern-CssSnippets) não oficial pode ajudá-lo a gerenciar e organizar seu CSS personalizado.

---

## Message Sound

Para reproduzir seu próprio som personalizado ao receber uma nova mensagem do bot, substitua o seguinte arquivo MP3 na sua pasta SillyTavern:

`public/sounds/message.mp3`

Reproduz em 80% de volume.

Se a opção "[Background Sound Only](index.md#miscellaneous)" estiver habilitada, o som toca apenas se a janela do SillyTavern estiver **sem foco**.

## Formulas Rendering

Para habilitar a renderização de fórmulas matemáticas, use a [extensão LaTeX](https://github.com/SillyTavern/Extension-LaTeX). Para obter a extensão, você precisa instalá-la através do menu "Download Extensions & Assets" no SillyTavern.

Digite suas fórmulas em blocos de código com identificadores de linguagem `latex` ou `asciimath` para LaTeX e AsciiMath respectivamente. A extensão usa [KaTeX](https://katex.org/) para renderização.

<pre><code>```latex
\int_{-\infty}^{\infty} e^{-x^2} dx = \sqrt{\pi}
```

```asciimath
int_{-oo}^{oo} e^{-x^2} dx = sqrt{pi}
```</code></pre>

!!!info Aviso de descontinuação
A sintaxe de wrapper legada `$` e `$$` não é mais suportada. Por favor, use os seguintes scripts regex para fazer polyfill da sintaxe antiga:

* [$$ - LaTeX](https://github.com/SillyTavern/Extension-LaTeX/raw/refs/heads/main/assets/$$_-_latex.json)
* [$ - AsciiMath](https://github.com/SillyTavern/Extension-LaTeX/raw/refs/heads/main/assets/$_-_asciimath.json)
!!!
