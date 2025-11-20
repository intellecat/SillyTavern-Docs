---
order: 10
route: /usage/api-connections/openrouter/
---

# OpenRouter

!!!info
OpenRouter está disponível como fonte de Text Completion e Chat Completion. Todos os modelos estão disponíveis através de qualquer API, mas seus recursos podem diferir dependendo do tipo de API que você escolher. Por exemplo, inlining de imagens e tool calling estão disponíveis apenas com a API de Chat Completion.
!!!

Não quer se inscrever em uma dúzia de serviços de API, mas ainda quer acesso a todos os modelos mais recentes? Use OpenRouter.

OpenRouter funciona permitindo que você use um único endpoint para acessar modelos como DeepSeek, Claude e Gemini, tudo em um serviço com um pool de créditos compartilhado.

Tem um teste gratuito (cerca de $1) e acesso pago depois. Sem assinatura ou conta mensal - você paga pelo que realmente usa. Alguns modelos têm acesso gratuito com um número limitado de solicitações diárias.

!!!tip
Para obter acesso permanente a modelos gratuitos com um limite diário generoso, você precisa comprar pelo menos $10 em créditos **uma vez**.

Veja mais detalhes na [página de FAQ do OpenRouter](https://openrouter.ai/docs/faq).
!!!

- Crie uma conta OpenRouter: [openrouter.ai](https://openrouter.ai/)
- [Lista de Modelos OpenRouter](https://openrouter.ai/models?order=pricing-low-to-high)

![OpenRouter-ConnectionPanel](/static/openrouter-connection.png)

De cima para baixo (veja a imagem acima):

1. Selecione a API 'Chat Completion'.
2. Selecione OpenRouter como a fonte.
3. Clique em "Authorize" para obter uma chave usando o fluxo OAuth. Alternativamente, gere uma chave de API [aqui](https://openrouter.ai/keys) e cole-a na caixa.
4. Clique em "Connect" e selecione um modelo.
5. (Opcional) Use o botão "Test Message" para verificar sua conexão.
