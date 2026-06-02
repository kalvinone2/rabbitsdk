# Hosting en GitHub

## Opcion recomendada

Repo privado para el codigo y GitHub Pages como URL publica no listada.

GitHub indica que Pages esta disponible en repos publicos con GitHub Free, y en repos publicos o privados con GitHub Pro, Team, Enterprise Cloud y Enterprise Server. Tambien avisa de que los sitios de Pages son publicos en internet aunque el repositorio sea privado, salvo configuraciones empresariales con Pages privado.

Eso encaja con Rabbit R1 porque el dispositivo necesita poder abrir la URL sin login.

## Pasos

1. Crear repo `rabbitsdk`.
2. Subir estos archivos.
3. Activar Pages desde `Settings > Pages`.
4. Publicar desde branch `main`, carpeta `/`.
5. Cambiar los JSON de `qr-data/` para apuntar a la URL real.
6. Generar QR y escanear desde `creations` en el R1.

## Si necesitas privacidad real

No uses una URL que requiera login para una Creation sencilla: el R1 puede no poder resolverla como web app instalable. Mejor:

- No incluir secretos.
- Usar URLs poco obvias.
- Mover cualquier API key a un backend.
- Usar Vercel/Netlify con backend o proxy si hace falta autenticacion.

