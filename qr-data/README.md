# Datos para QR de Rabbit R1 Creations

Estos JSON usan el formato del generador oficial `rabbit-hmi-oss/creations-sdk/qr/final`:

- `title`
- `url`
- `description`
- `iconUrl`
- `themeColor`

Antes de generar los QR, cambia `https://kalvinone2.github.io/rabbitsdk` si publicas este proyecto con otro usuario, organizacion o nombre de repo.

La app `n8n-chat-r1` necesita un segundo QR privado dentro de la app para configurar el webhook. Ese QR no debe subirse al repo y puede tener este formato:

```json
{"endpoint":"https://tu-n8n.com/webhook/rabbit-chat","token":"r1_token_privado","name":"n8n personal"}
```
