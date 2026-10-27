# Kifu — repaso de líneas de Go

App de una sola carpeta, sin backend ni dependencias externas. Todo se guarda en `localStorage` del navegador, en tu propio dispositivo.

## Secciones

- **Tsumegos** — *Repasar* (repetición espaciada con límite diario), *Libre* (resolución libre en orden alfabético o aleatorio, con anterior/siguiente e «Ir a», sin límite y sin afectar el calendario), *Tarjetas* (carpetas, marcas ★ y ?, editar) y *Crear* (tablero, árbol de variantes, ramas correcta/incorrecta, importar imagen/SGF).
- **Partidas** — *Mis partidas*, *Nueva* e *Importar* (SGF). La lista de partidas tiene búsqueda (título o jugador), carpetas, orden y filtro de marcadas. Cada partida (siempre editable, con ★ de marca y los datos en un desplegable bajo el tablero) guarda título, jugadores (con color), kyu/dan, komi, handicap, fecha, resultado y un comentario por jugada. Botón **Pasar** en el registro; al tocar un punto el tablero se acerca a esa zona y se confirma con un segundo toque en el mismo punto (o «Confirmar jugada»; «Cancelar» lo deshace). Visor con árbol horizontal, que solo baja a otra fila cuando una rama chocaría con otra; no entran en el repaso.
- **Josekis** — *Biblioteca* (un archivo por joseki, agrupados en carpetas; importa varios .sgf, un .zip o una carpeta del dispositivo — sus subcarpetas pasan a ser carpetas; cada joseki es un árbol SGF con opciones A, B, C…, pérdida de puntos y comentario por jugada, botón «+ Tenuki» (pasar): crea una rama donde quien juega pasa y sigues con la secuencia local que resulta, «Tenuki» la abre si ya existe, se guarda como `;B[]`/`;W[]` en el SGF; exportar .sgf por archivo) y *Practicar* (puedes practicar toda la biblioteca, una carpeta o un archivo; la máquina responde al azar entre tus ramas).
- **Ajustes** — recorte automático, coordenadas, límite diario, almacenamiento y variantes.

## Jugar en repaso y práctica

Toca un punto para seleccionarlo; doble toque (o **Confirmar jugada**) para jugarlo. En josekis: **Tenuki** pasa: sigue la rama de tenuki de tu biblioteca (si no existe cuenta como fallo), **Ver opciones** muestra las letras, En partidas y josekis las marcas (triángulo, cuadrado, letra y número) usan el mismo selector que el editor de tsumegos y se guardan por posición en el SGF (TR/SQ/LB). Partidas, josekis y el editor de tsumegos muestran las últimas 10 jugadas sobre el tablero (‹ muestra el resto). Tocar fuera del tablero deshace la jugada seleccionada. En josekis puedes marcar jugadas con ★. **Atrás** retrocede 2 posiciones (tu jugada y la respuesta; 1 si juegas con ambos colores) y **Reiniciar** vuelve al inicio.

## Variantes (Ajustes)

Para tsumegos y para josekis por separado: **posiciones espejo y rotadas** (las 8 simetrías, incluidos los espejos en diagonal) y **colores invertidos**. Se sortean en cada repaso o práctica; las tarjetas y la biblioteca originales no cambian. Si la posición es simétrica, una jugada equivalente por simetría también se acepta, en el repaso de tsumegos y en la práctica de josekis.

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

## Guardado automático

Tarjetas, partidas y josekis se guardan solos al editar; no hay botón de guardar obligatorio (en tarjetas, «Guardar y siguiente» solo sirve para pasar a la siguiente). Los avisos de «guardado» e importación ya no aparecen; solo se muestran los errores (jugada ilegal, archivo ilegible).

## Respaldo

Ajustes → **Respaldo completo** exporta un .json con todo (tarjetas con su calendario, partidas, biblioteca de josekis y ajustes) y lo restaura en otro navegador. Las partidas ya guardadas y la biblioteca se autoguardan al editar.

## Nota sobre los datos

Todo lo que crees (tarjetas, repasos, ajustes) se guarda con `localStorage`, **en ese navegador y en ese dispositivo únicamente**. No hay servidor ni sincronización — para pasar tus tarjetas a otro navegador o dispositivo, usa las opciones de exportar/importar (.zip, .sgf, o copiar y pegar como texto) que están dentro de la pestaña "Ajustes" de la app.
