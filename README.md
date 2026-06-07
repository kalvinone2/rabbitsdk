# Rabbit R1 Creations sandbox

Dos apps de prueba para validar el SDK de Rabbit R1 Creations en hardware real.

## Apps

- `apps/fortune-wheel/`: ruleta absurda con scroll wheel, PTT, acelerometro, almacenamiento local y respuesta hablada opcional.
- `apps/excuse-bot/`: generador de excusas tontas usando el LLM del R1, PTT, scroll wheel, almacenamiento y TTS.
- `apps/instantsfun-r1/`: mini soundboard de Instantsfun con 424 botones precargados, rejilla 2 columnas, reproduccion directa de MP3, shake para parar y boton lateral random.
- `apps/n8n-chat-r1/`: chat ligero para conectar Rabbit R1 a n8n mediante un QR privado con endpoint y token.
- `apps/drop-board-r1/`: tablero de drops por area conectado a n8n y Notion, con instrucciones por PTT y refresco automatico del listado.

## Desarrollo local

Estas apps son HTML/CSS/JS estatico sin build.

```bash
python3 -m http.server 8080
```

Despues abre:

- `http://localhost:8080/apps/fortune-wheel/`
- `http://localhost:8080/apps/excuse-bot/`
- `http://localhost:8080/apps/instantsfun-r1/`
- `http://localhost:8080/apps/n8n-chat-r1/`
- `http://localhost:8080/apps/drop-board-r1/`

En navegador normal veras fallbacks porque las APIs `PluginMessageHandler`, `creationStorage` y `creationSensors` solo existen dentro del R1.

## Hosting

Para instalarlas en Rabbit R1 necesitas URLs HTTPS publicas:

- GitHub Pages, Netlify o Vercel funcionan bien.
- Puedes guardar el codigo en un repo privado, pero la pagina publicada debe ser accesible por el R1.
- En GitHub Pages, los sitios publicados desde repos privados dependen del plan; ademas, la web publicada puede seguir siendo publica aunque el repo sea privado.
- No pongas API keys ni secretos en frontend. Usa backend propio si alguna app necesita credenciales.

## Instalacion en R1

1. Publica este directorio como web estatica.
2. Sustituye la URL de los JSON de `qr-data/` si el repo no se publica como `https://kalvinone2.github.io/rabbitsdk`.
3. Genera QR con la herramienta `qr/final` del repo oficial `rabbit-hmi-oss/creations-sdk`.
4. En el R1 abre `creations`, elige `add via QR code` y escanea.

El generador oficial espera exactamente estos campos: `title`, `url`, `description`, `iconUrl`, `themeColor`.

Tambien hay QR ya generados en `qr/` y publicados en `https://kalvinone2.github.io/rabbitsdk/qr/`.

## URLs esperadas

Si publicas en GitHub Pages como proyecto:

- `https://kalvinone2.github.io/rabbitsdk/apps/fortune-wheel/`
- `https://kalvinone2.github.io/rabbitsdk/apps/excuse-bot/`
- `https://kalvinone2.github.io/rabbitsdk/apps/instantsfun-r1/`
- `https://kalvinone2.github.io/rabbitsdk/apps/n8n-chat-r1/`
- `https://kalvinone2.github.io/rabbitsdk/apps/drop-board-r1/`

## Configuracion privada de n8n Chat R1 / Mind Drop

La app `n8n-chat-r1` es publica, pero no contiene secretos. En el primer arranque escanea un QR privado con:

```json
{"endpoint":"https://tu-n8n.com/webhook/rabbit-r1-notion-v0","token":"r1_token_privado","name":"Mind Drop","lang":"es-ES","speak":true}
```

El token se guarda en `creationStorage.secure` cuando esta disponible, con fallback a `localStorage` en navegador normal. Las peticiones a n8n se envian como `application/x-www-form-urlencoded` con un campo `payload` JSON para reducir problemas de CORS.

En n8n, si quieres revocar un token, responde con `{"revoked":true}`, `{"status":"revoked"}` o HTTP `401/403`; la app borrara la configuracion local y volvera al setup.

Hay un workflow importable de prueba en `workflows/rabbit-r1-notion-v0.workflow.json`. Recibe texto desde el Rabbit, valida token, clasifica el mensaje con `gpt-4.1-nano` y crea una fila en la BBDD `Drops` de Notion dentro de `Mind Drop`.

El workflow publico usa placeholders para secretos:

- `REPLACE_WITH_RABBIT_TOKEN`
- `REPLACE_WITH_OPENAI_API_KEY`
- `REPLACE_WITH_NOTION_CREDENTIAL_ID`

Para evitar que el lector QR del R1 crashee la app, `n8n-chat-r1` tambien soporta configurar por PIN contra `https://n8n.calvonavarro.com/webhook/rabbit-r1-config-pin`. El workflow importable esta en `workflows/rabbit-r1-config-pin.workflow.json` y devuelve la misma config privada que antes iba dentro del QR.

## Generador QR

El repo oficial contiene el generador en `qr/final`, pero la URL de GitHub Pages del proyecto no esta publicada actualmente. Opciones practicas:

- Clonar `https://github.com/rabbit-hmi-oss/creations-sdk` y abrir/hostear `qr/final/index.html`.
- Copiar la carpeta `qr/final` a este repo si quieres tener el generador dentro del mismo proyecto.
- Pegar manualmente los valores de `qr-data/*.json` en el formulario del generador.
