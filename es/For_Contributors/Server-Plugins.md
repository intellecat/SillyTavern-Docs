---
order: -30
icon: server
route: /for-contributors/server-plugins/
---

# Complementos de Servidor

Estos complementos permiten agregar funcionalidad que es imposible lograr solo con extensiones de UI, como crear nuevos puntos finales de API o usar paquetes de Node.JS que no están disponibles en un entorno de navegador.

Los complementos se encuentran en el directorio `plugins` de SillyTavern y se cargan en el inicio del servidor, pero *solo* si `enableServerPlugins` está configurado en `true` en el archivo `config.yaml`.

!!!warning Advertencia
 **Los complementos de servidor no están aislados. Esto significa que potencialmente pueden acceder a todo su sistema de archivos, o introducir una amplia gama de vulnerabilidades de seguridad de una manera que las extensiones normales de UI no pueden. ¡Solo instale complementos de servidor de desarrolladores en los que confíe!**
!!!

Para una lista de todos los complementos de servidor oficiales, consulte la lista de la organización GitHub: <https://github.com/search?q=topic%3Aplugin+org%3ASillyTavern&type=Repositories>

## Tipos de complementos

### Archivos

Un archivo JavaScript ejecutable con extensión ".js" (para módulos CommonJS) o ".mjs" (para módulos ES) que contiene un módulo que exporta una función `init`. Esta función acepta un enrutador Express (creado específicamente para su complemento) como argumento y devuelve una Promise.

El módulo también debe exportar un objeto `info` que contenga información sobre el complemento (cadenas de caracteres `id`, `name` y `description`). Esto proporcionará información sobre el complemento al cargador.

Puede registrar rutas a través del enrutador que se registrarán en la ruta `/api/plugins/{id}/{route}`. Por ejemplo, `router.get('/foo')` para el complemento `example` producirá una ruta como esta: `/api/plugins/example/foo`.

Un complemento puede *opcionalmente* exportar una función `exit` que realiza la limpieza al apagar el servidor. No debe tener argumentos y debe devolver una Promise.

Contrato de TypeScript para exportaciones de complementos:

```ts
interface PluginInfo {
    id: string;
    name: string;
    description: string;
}

interface Plugin {
    init: (router: Router) => Promise<void>;
    exit: () => Promise<void>;
    info: PluginInfo;
}
```

Vea a continuación un ejemplo de complemento "¡Hola mundo!":

```js
/**
 * Initialize plugin.
 * @param {import('express').Router} router Express router
 * @returns {Promise<any>} Promise that resolves when plugin is initialized
 */
async function init(router) {
    // Do initialization here...
    router.get('/foo', req, res, function () {
       res.send('bar');
    });
    console.log('Example plugin loaded!');
    return Promise.resolve();
}

async function exit() {
    // Do some clean-up here...
    return Promise.resolve();
}

module.exports = {
    init,
    exit,
    info: {
        id: 'example',
        name: 'Example',
        description: 'My cool plugin!',
    },
};
```

### Directorios

Puede cargar un complemento de un subdirectorio en el directorio `plugins` de una de las siguientes formas (en orden de precedencia):

1. Un archivo `package.json` que contiene una ruta a un archivo ejecutable en el campo "main".
2. Un archivo `index.js` para módulos CommonJS.
3. Un archivo `index.mjs` para módulos ES.

Un archivo resultante debe exportar una función `init` y un objeto `info` con los mismos requisitos que para archivos individuales.

Ejemplo de un complemento de directorio (con un archivo `index.js`): <https://github.com/SillyTavern/SillyTavern-DiscordRichPresence-Server>

### Agrupación

Es preferible usar un empaquetador (como Webpack o Browserify) que empaquete todos los requisitos en un archivo. Asegúrese de establecer "Node" como destino de compilación.

Repositorio de plantilla para complementos que utilizan Webpack y TypeScript: <https://github.com/SillyTavern/Plugin-WebpackTemplate>
