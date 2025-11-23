---
route: /usage/api-connections/tabbyapi/
---

# TabbyAPI
Una aplicación basada en FastAPI que permite generar texto usando un LLM con el backend Exllamav2, con soporte para modelos Exl2, GPTQ y FP16.

* [GitHub](https://github.com/theroyallab/tabbyAPI)

### Inicio Rápido
1. Sigue las [instrucciones de instalación](https://github.com/theroyallab/tabbyAPI/wiki/01.-Getting-Started) en el GitHub oficial de TabbyAPI.
2. [Crea tu config.yml](https://github.com/theroyallab/tabbyAPI/wiki/02.-Server-options) para establecer la ruta de tu modelo, modelo predeterminado, longitud de secuencia, etc. Puedes ignorar la mayoría (si no todos) de estos ajustes si lo deseas.
3. Inicia TabbyAPI. Si funcionó, deberías ver algo como esto:

    ![TabbyAPI terminal](/static/tabby-terminal.png)

4. En la API de Finalización de Texto en SillyTavern, selecciona TabbyAPI.
5. Copia tu clave API de la terminal de TabbyAPI a `Tabby API key` y asegúrate de que tu `API URL` sea correcta (debería ser `http://127.0.0.1:5000` de forma predeterminada).

Si lo hiciste todo correctamente, deberías ver algo como esto en SillyTavern:

![TabbyAPI SillyTavern](/static/tabby-config.png)

¡Ahora puedes chatear usando TabbyAPI!

### Cargador de TabbyAPI
Los desarrolladores de TabbyAPI crearon una extensión oficial para cargar/descargar modelos directamente desde SillyTavern. La instalación es simple:
1. En SillyTavern, haz clic en la pestaña Extensiones y navega a Descargar Extensiones y Recursos.
2. Copia `https://raw.githubusercontent.com/theroyallab/ST-repo/main/index.json` en Assets URL y haz clic en el botón de enchufe a la derecha.
3. Deberías ver algo como esto. Haz clic en el botón de descarga junto a Tabby Loader.

    ![Tabby Loader](/static/tabby-assets.png)

4. Si la instalación fue exitosa, deberías ver un mensaje emergente verde en la parte superior de tu pantalla. En la pestaña de extensiones, navega a TabbyAPI Loader y copia tu clave de administrador de la terminal de TabbyAPI a Admin Key.
5. Haz clic en el botón de actualización junto a Model Select. Cuando hagas clic en el cuadro de texto justo debajo, deberías ver todos los modelos en tu directorio de modelos.

![Tabby Loader Extension](/static/tabby-loader.png)

¡Ahora puedes cargar y descargar tus modelos directamente desde SillyTavern!

### Soporte
¿Aún necesitas ayuda? Visita el [GitHub de TabbyAPI](https://github.com/theroyallab/tabbyAPI) para encontrar un enlace al servidor oficial de Discord del desarrollador y [lee la wiki](https://github.com/theroyallab/tabbyAPI/wiki/1.-Getting-Started).
