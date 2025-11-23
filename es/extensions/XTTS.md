---
order: tts-xtts
route: /extensions/xtts/
---

# XTTS con clonación de voz

¡Saludos! Así que, ¿has quedado asombrado por esos posts de Reddit que muestran cuán lejos ha llegado la tecnología en síntesis de voz de IA?

¿Te emocionaría dar a tu robótica waifu/husbando un nuevo y reluciente modulador de voz?

No temas, esta asombrosa tecnología revolucionaria ya está disponible en tu SillyTavern local, solo necesitas algo simple...

## Requisitos previos

1. La última versión de SillyTavern.
2. [Miniconda](https://docs.conda.io/projects/miniconda/en/latest/miniconda-install.html) instalada.
3. (Windows) [Visual C++ Build Tools](https://visualstudio.microsoft.com/visual-cpp-build-tools/) instaladas.
4. Archivos WAV con clips de voz para clonar (~10 segundos por archivo). Requisitos del archivo: PCM, Mono, 22050Hz, 16-bit (convertir a través de Audacity).
5. Crear una carpeta con subcarpetas "speakers" y "output". Coloca archivos WAV en "speakers".

Estructura de carpeta de ejemplo:
```
C:\xtts
  - speakers
    - alice.wav
    - bob.wav
  - output
```

## Instalando

[daswer123](https://github.com/daswer123) creó un servidor API que ejecuta el modelo XTTSv2 en tu computadora y se conecta a la extensión TTS de SillyTavern.

Es completamente independiente de Extras API y usaría un entorno separado.

**Muy importante:** No instales los siguientes requisitos en tu entorno de Extras o en el Python del sistema.
Romperá tus otros paquetes y causará degradaciones innecesarias, etc.

Las siguientes instrucciones se proporcionan usando Miniconda, pero también puedes hacerlo con venv (no cubierto aquí).
Abre el símbolo del sistema de Anaconda y sigue las instrucciones línea por línea.

### Poner el servidor en funcionamiento

1. Navega a la carpeta que creaste en el paso 4 de requisitos previos.
    ```
    cd C:\xtts
    ```
2. Crea un nuevo entorno de conda. De ahora en adelante, lo llamaremos `xtts`.
    ```
    conda create -n xtts
    ```
3. Activa el entorno recién creado.
    ```
    conda activate xtts
    ```
4. Instala Python 3.10 en tu entorno. Confirma con "y" cuando se te pida.
    ```
    conda install python=3.10
    ```
5. Instala el servidor XTTS con sus requisitos.
    ```
    pip install xtts-api-server pydub
    ```
6. Instala PyTorch. Esto puede tomar algo de tiempo. La siguiente línea instala PyTorch con soporte de aceleración GPU (CUDA).
Si deseas usar solo la inferencia de CPU, elimina la última parte que comienza con `--index-url`.
    ```
    pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
    ```
7. Inicia el servidor XTTS en el host y puerto predeterminados: <http://localhost:8020>
    ```
    python -m xtts_api_server
    ```
8. Durante tu primer inicio, el modelo se descargará (~2 GB).
No olvides leer el aviso legal de Coqui AI muy cuidadosamente. Jaja, estoy bromeando, solo presiona "y" de nuevo.

### Conectando a SillyTavern

1. Abre el panel de extensiones, expande el menú TTS y elige "XTTSv2" en la lista de proveedores.
2. Elige tu idioma de síntesis de voz en el menú desplegable de idioma (Estaré triste si no es polaco).
3. Verifica que el endpoint del proveedor apunte a <http://localhost:8020> y que "Voces disponibles" muestre una lista de tus muestras de voz.
4. Elige cualquier personaje y establece un mapeo entre la muestra de voz y el personaje.
Si la lista de personajes está vacía, presiona "Recargar" un par de veces.
5. Configura el resto de los ajustes de TTS según tus preferencias.

### ¡Ya estás listo!

Haz clic en el icono de altavoz en el menú de acciones de contexto para cualquier mensaje y escucha la hermosa voz clonada emanando
de tus altavoces. La generación toma algo de tiempo y no es en tiempo real ni siquiera en GPUs RTX de alta gama.

### ¿Streaming?

¡Es posible usar streaming HTTP con la última versión del servidor XTTS para obtener fragmentos de audio generado tan pronto como esté disponible!

#### ¡Esto no funciona con RVC!

El audio aún se generará (asumiendo que estés usando la última versión de la extensión RVC) y se convertirá, *pero no se transmitirá en streaming* ya que RVC requiere tener el archivo de audio completo antes de iniciar la conversión. El RVC transmitido en streaming aún se está investigando...

#### ¿Cómo obtener soporte de streaming?

1. Actualiza SillyTavern a la última versión.
2. Actualiza el servidor XTTS a la última versión.

    ```bash
    conda activate xtts
    pip install xtts-api-server --upgrade
    ```

3. Inicia y conecta XTTS a ST como de costumbre.
4. Habilita la configuración de extensión "Streaming" de XTTS en SillyTavern.

### ¿Audio entrecortado?

Intenta aumentar la configuración de "tamaño de fragmento".

Como referencia: con un tamaño de fragmento de 200, RTX 3090 puede producir audio ininterrumpido al costo de una mayor latencia de audio.

### ¿Cómo reiniciar el servidor TTS?

Solo haz los pasos 1, 3 y 7 de la instrucción de instalación.

### ¿Android??

Improbable, no puede ejecutar aplicaciones que requieran PyTorch sin algún tipo de magia arcana para la que no proporcionamos soporte. Puedes intentarlo bajo tu propio riesgo, pero no se proporcionará soporte si enfrentas algún problema.

Tu mejor solución es alojar el API de TTS en tu PC a través de la red local, solo no olvides especificar el host y el puerto para escuchar - ver [README](https://github.com/daswer123/xtts-api-server/blob/main/README.md).
