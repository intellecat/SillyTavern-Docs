---
order: 10
route: /usage/api-connections/openrouter/
---

# OpenRouter

!!!info
OpenRouter está disponible como fuente de Text Completion y Chat Completion. Todos los modelos están disponibles a través de cualquiera de los API, pero sus características pueden variar según el tipo de API que elijas. Por ejemplo, la inserción de imágenes y tool calling solo están disponibles con el API de Chat Completion.
!!!

¿No quieres registrarte en una docena de servicios de API, pero aún quieres acceso a todos los últimos modelos? Usa OpenRouter.

OpenRouter funciona permitiéndote usar un único endpoint para acceder a modelos como DeepSeek, Claude y Gemini, todos en un servicio con un pool de créditos compartido.

Tiene una prueba gratuita (aproximadamente $1) y acceso de pago después. Sin suscripción ni factura mensual: pagas por lo que realmente usas. Algunos modelos tienen acceso gratuito con un número limitado de solicitudes diarias.

!!!tip
Para obtener acceso permanente a modelos gratuitos con un límite diario generoso, debes comprar al menos $10 en créditos **una vez**.

Ver más detalles en la [página de OpenRouter FAQ](https://openrouter.ai/docs/faq).
!!!

- Crear una cuenta de OpenRouter: [openrouter.ai](https://openrouter.ai/)
- [Lista de Modelos de OpenRouter](https://openrouter.ai/models?order=pricing-low-to-high)

![OpenRouter-ConnectionPanel](/static/openrouter-connection.png)

De arriba a abajo (ver imagen arriba):

1. Selecciona el API de 'Chat Completion'.
2. Selecciona OpenRouter como la fuente.
3. Haz clic en 'Authorize' para obtener una clave usando el flujo OAuth. Alternativamente, genera una clave API [aquí](https://openrouter.ai/keys) y pégala en el cuadro.
4. Haz clic en 'Connect' y selecciona un modelo.
5. (Opcional) Usa el botón 'Test Message' para verificar tu conexión.
