# Kifu — repaso de líneas de Go

App de una sola carpeta, sin backend ni dependencias externas. Todo se guarda en `localStorage` del navegador, en tu propio dispositivo.

## Archivos

```
index.html              la app completa (HTML + CSS + JS)
manifest.webmanifest     metadatos de la PWA (nombre, ícono, colores)
service-worker.js        cachea la app para que funcione sin conexión y la hace instalable
icons/icon-192.png
icons/icon-512.png
icons/apple-touch-icon.png
```

## Cómo publicarlo con GitHub Pages (gratis)

1. Crea un repositorio nuevo en GitHub (puede ser público o privado si tienes GitHub Pro).
2. Sube estos archivos tal cual, manteniendo la carpeta `icons/` — por ejemplo arrastrándolos en la pestaña **Add file → Upload files** del repositorio, o con `git push` si usas la terminal.
3. Ve a **Settings → Pages** del repositorio.
4. En "Build and deployment", elige **Deploy from a branch**, rama `main` (o `master`), carpeta `/ (root)`. Guarda.
5. Espera uno o dos minutos — GitHub te dará una URL como:
   `https://tu-usuario.github.io/tu-repositorio/`
6. Abre esa URL en Chrome (celular o computadora). Como ahora es HTTPS con manifest y service worker, Chrome debería ofrecer **"Instalar app"** de verdad (ícono propio, ventana propia, sin la barra del navegador).

## Actualizar la app más adelante

Cada vez que subas cambios nuevos a `index.html`, sube también un cambio a `service-worker.js` — basta con subir el número de la constante `VERSION` al principio del archivo (por ejemplo de `'kifu-v1'` a `'kifu-v2'`). Sin ese cambio, el service worker puede seguir sirviendo la versión vieja desde caché por un tiempo.

## Nota sobre los datos

Todo lo que crees (tarjetas, repasos, ajustes) se guarda con `localStorage`, **en ese navegador y en ese dispositivo únicamente**. No hay servidor ni sincronización — para pasar tus tarjetas a otro navegador o dispositivo, usa las opciones de exportar/importar (.zip, .sgf, o copiar y pegar como texto) que están dentro de la pestaña "Ajustes" de la app.
