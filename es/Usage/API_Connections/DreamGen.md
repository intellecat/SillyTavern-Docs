---
route: /usage/api-connections/dreamgen/
---

# DreamGen

DreamGen es una aplicación y una API para roleplay impulsado por IA y escritura de historias. Tienen un nivel gratuito, así como una suscripción de pago que permite acceso ilimitado mensual a sus modelos de generación de texto de alta calidad diseñados específicamente para roleplay de IA dirigible y escritura de historias. Crea una cuenta para comenzar: <https://dreamgen.com/>.

Los créditos (gratuitos) se restablecen al comienzo de cada mes del calendario. Consulta [pricing](https://dreamgen.com/pricing) para ver el costo de créditos para cada modelo e [usage](https://dreamgen.com/account/usage) para ver tus créditos restantes.

## Conectando a DreamGen

### Obtener clave API

Ve a la página [DreamGen API keys](https://dreamgen.com/account/api-keys) y haz clic en el botón "New API Key". Asegúrate de que la clave API se copie en tu portapapeles.

![Create New DreamGen API key](/static/dreamgen/dreamgen_api_keys_new.jpg)
![Copy DreamGen API key](/static/dreamgen/dreamgen_api_keys_copy.jpg)

### Conectar

1. Ve a la configuración de conexión de SillyTavern.
2. Selecciona API: Text Completion
3. Selecciona API Type: DreamGen
4. Ingresa la clave API
5. (opcional) Elige un modelo

![Connecting to DreamGen](/static/dreamgen/dreamgen_st_connection.png)

## Modelos

La API de DreamGen ofrece varios modelos de diferentes tamaños.

- Lucid Max (en la API llamado `lucid-v1-max` o `lucid-v1-extra-large`)
- Lucid Base (en la API llamado `lucid-v1-base` o `lucid-v1-medium`) -- corresponde a la versión de peso disponible [Lucid V1 Nemo](https://dreamgen.com/docs/models/lucid-v1/huggingface).

Lucid Base utiliza muchos menos créditos y es más rápido, mientras que Lucid Max es más creativo y puede manejar instrucciones y narrativas más complejas.

## Configuración

Los modelos Lucid V1 DreamGen utilizan una extensión de la plantilla de chat Llama 3 optimizada para roleplay y escritura. Funcionan mejor con un aviso del sistema específico.

Recomendamos encarecidamente comenzar con uno de estos presets maestros:

- Asegúrate de que tengas el modo instruct habilitado y selecciona todas las casillas al importar.
- [DreamGen Lucid V1 Role-Play preset](https://dreamgen.com/docs/models/lucid-v1/sillytavern/master-preset/role-play)
- [DreamGen Lucid V1 Story preset](https://dreamgen.com/docs/models/lucid-v1/sillytavern/master-preset/story)

Estos presets vienen con soporte integrado para `/sys` para enviar instrucciones al modelo. Puedes usarlos para dirigir la trama o controlar las acciones de los personajes.

![DreamGen preset selected](/static/dreamgen/dreamgen_st_preset.png)

Otros recursos:

- [**Guía detallada de DreamGen + SillyTavern**](https://dreamgen.com/docs/models/lucid-v1/sillytavern)
- [Documentación detallada del formato de aviso Lucid V1](https://dreamgen.com/docs/models/lucid-v1).
- [Demo de roleplay de DreamGen + SillyTavern](https://imgur.com/a/dreamgen-lucid-sillytavern-roleplay-demo-bhzQpto)
- [Demo de escritura de historias de DreamGen + SillyTavern](https://imgur.com/a/dreamgen-lucid-sillytavern-writing-demo-JLv5iO3)
- [Consejos para crear tus propios escenarios](https://v2.dreamgen.com/docs/scenario-editor)

## Preguntas Frecuentes

### ¿Cómo puedo hacer que las respuestas sean más largas o más cortas?

Puedes establecer el `Last Assistant Prefix` en tus presets de formato.

Para mensajes largos:

```txt
<|start_header_id|>user<|end_header_id|>

The next message is from {{char}} and is at least 100 words long<|eot_id|><|start_header_id|>writer character {{char}}<|end_header_id|>

```

Para mensajes cortos:

```txt
<|start_header_id|>user<|end_header_id|>

The next message is from {{char}} and is at most 50 words long<|eot_id|><|start_header_id|>writer character {{char}}<|end_header_id|>

```

Asegúrate de preservar todos los saltos de línea, incluidos los dos al final.

![Long Message Prefix](/static/dreamgen/dreamgen_st_long_response_prefix.png)

También puedes incluir la descripción del estilo de escritura en tu tarjeta o aviso del sistema, por ejemplo:

```txt
## Style

<your description>
```

Consulta la documentación ["Style"](https://v2.dreamgen.com/docs/scenario-editor#style) para obtener más información y ver algunos ejemplos.

### ¿Cómo puedo dirigir el roleplay / historia?

Usa la opción `/sys` para enviar instrucciones al modelo. Algunos ejemplos:

> The inkeeper offers Daria and the others a pint of ale.

> The next message is from Draco and should be at least 200 words, focusing on his inner conflict about the decision.

[Vélo en acción.](https://imgur.com/a/dreamgen-lucid-sillytavern-roleplay-demo-bhzQpto)
