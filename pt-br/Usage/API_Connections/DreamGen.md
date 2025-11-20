---
route: /usage/api-connections/dreamgen/
---

# DreamGen

DreamGen é um aplicativo e uma API para role-playing e escrita de histórias com IA. Eles têm uma camada gratuita, bem como uma assinatura paga que permite acesso mensal ilimitado aos seus modelos de geração de texto internos de alta qualidade feitos especificamente para o propósito de role-playing de IA dirigível e escrita de histórias. Crie uma conta para começar: <https://dreamgen.com/>.

Os créditos (gratuitos) são redefinidos no início de cada mês do calendário. Veja [pricing](https://dreamgen.com/pricing) para ver o custo de crédito para cada modelo e [usage](https://dreamgen.com/account/usage) para ver seus créditos restantes.

## Conectando ao DreamGen

### Obter Chave de API

Vá para a página [DreamGen API keys](https://dreamgen.com/account/api-keys) e clique no botão "New API Key". Certifique-se de que a API Key foi copiada para sua área de transferência.

![Create New DreamGen API key](/static/dreamgen/dreamgen_api_keys_new.jpg)
![Copy DreamGen API key](/static/dreamgen/dreamgen_api_keys_copy.jpg)

### Conectar

1. Vá para as configurações de conexão do SillyTavern.
2. Selecione API: Text Completion
3. Selecione API Type: DreamGen
4. Insira a chave de API
5. (opcional) Escolha um modelo

![Connecting to DreamGen](/static/dreamgen/dreamgen_st_connection.png)

## Modelos

A API DreamGen oferece vários modelos de diferentes tamanhos.

- Lucid Max (na API chamado `lucid-v1-max` ou `lucid-v1-extra-large`)
- Lucid Base (na API chamado `lucid-v1-base` ou `lucid-v1-medium`) -- corresponde ao peso disponível [Lucid V1 Nemo](https://dreamgen.com/docs/models/lucid-v1/huggingface).

Lucid Base usa muito menos créditos e é mais rápido, enquanto Lucid Max é mais criativo e é capaz de lidar com instruções e narrativas mais complexas.

## Configurações

Os modelos DreamGen Lucid V1 usam uma extensão do template de chat Llama 3 otimizado para role-play e escrita. Eles funcionam melhor com um system prompt específico.

Recomendamos fortemente começar com uma destas predefinições mestras:

- Certifique-se de ter o modo instruct habilitado e selecione todas as caixas de seleção ao importar.
- [DreamGen Lucid V1 Role-Play preset](https://dreamgen.com/docs/models/lucid-v1/sillytavern/master-preset/role-play)
- [DreamGen Lucid V1 Story preset](https://dreamgen.com/docs/models/lucid-v1/sillytavern/master-preset/story)

Essas predefinições vêm com suporte integrado para `/sys` para enviar instruções ao modelo. Você pode usá-las para direcionar o enredo ou controlar as ações do personagem.

![DreamGen preset selected](/static/dreamgen/dreamgen_st_preset.png)

Outros recursos:

- [**Guia detalhado DreamGen + SillyTavern**](https://dreamgen.com/docs/models/lucid-v1/sillytavern)
- [Documentação detalhada do formato de prompt Lucid V1](https://dreamgen.com/docs/models/lucid-v1).
- [Demo de role-play DreamGen + SillyTavern](https://imgur.com/a/dreamgen-lucid-sillytavern-roleplay-demo-bhzQpto)
- [Demo de escrita de histórias DreamGen + SillyTavern](https://imgur.com/a/dreamgen-lucid-sillytavern-writing-demo-JLv5iO3)
- [Dicas para fazer seus próprios cenários](https://v2.dreamgen.com/docs/scenario-editor)

## FAQ

### Como posso fazer as respostas mais longas ou mais curtas?

Você pode definir o `Last Assistant Prefix` em suas predefinições de formatação.

Para mensagens longas:

```txt
<|start_header_id|>user<|end_header_id|>

The next message is from {{char}} and is at least 100 words long<|eot_id|><|start_header_id|>writer character {{char}}<|end_header_id|>

```

Para mensagens curtas:

```txt
<|start_header_id|>user<|end_header_id|>

The next message is from {{char}} and is at most 50 words long<|eot_id|><|start_header_id|>writer character {{char}}<|end_header_id|>

```

Certifique-se de preservar todas as quebras de linha, incluindo as duas no final.

![Long Message Prefix](/static/dreamgen/dreamgen_st_long_response_prefix.png)

Você também pode incluir descrição de estilo de escrita no seu card ou system prompt, por exemplo:

```txt
## Style

<your description>
```

Veja a [documentação de "Style"](https://v2.dreamgen.com/docs/scenario-editor#style) para aprender mais e ver alguns exemplos.

### Como posso direcionar o role-play / história?

Use a opção `/sys` para enviar instruções ao modelo. Alguns exemplos:

> The inkeeper offers Daria and the others a pint of ale.

> The next message is from Draco and should be at least 200 words, focusing on his inner conflict about the decision.

[Veja em ação.](https://imgur.com/a/dreamgen-lucid-sillytavern-roleplay-demo-bhzQpto)
