# Kifu — repaso de líneas de Go

App de una sola carpeta, sin backend ni dependencias externas. Todo se guarda en la base de datos del navegador (IndexedDB, cientos de MB o más según el dispositivo), en tu propio dispositivo. Si el navegador no la ofrece, usa `localStorage` (~5 MB) o la memoria de la sesión.

## Secciones

- **Tsumegos** — *Repasar* (repetición espaciada con límite diario), *Libre* (resolución libre; el selector de carpeta de Repasar y Libre solo lista las carpetas habilitadas en Ajustes y filtra solo durante la sesión, sin cambiar esa configuración; en orden alfabético o aleatorio, con anterior/siguiente, un contador «12/340» que se toca para escribir el número al que saltar y un buscador por título cuyos resultados salen siempre en orden alfabético; sin límite y sin afectar el calendario), *Tarjetas* (carpetas —con botón para eliminar una carpeta completa, con doble toque de confirmación—, marcas ★ y ?, editar) y *Crear* (tablero, árbol de variantes, ramas correcta/incorrecta, importar SGF).
- **Partidas** — *Mis partidas*, *Nueva* e *Importar* (SGF). Como en tsumegos y josekis, Ajustes → Repaso tiene la lista plegable «Carpetas de partidas a mostrar»: las partidas de carpetas desactivadas se ocultan de la lista (un aviso indica cuántas), y la lista permite eliminar una carpeta completa con doble toque. La lista de partidas tiene búsqueda (título o jugador), carpetas, orden y filtro de marcadas. Cada partida (tablero 9×9, 13×13 o 19×19 desde el desplegable de datos; siempre editable, con ★ de marca y los datos en un desplegable bajo el tablero) guarda título, jugadores (con color), kyu/dan, komi, handicap, fecha, resultado y un comentario por jugada. Botón **Pasar** en el registro; al tocar un punto el tablero se acerca a esa zona y se confirma con un segundo toque en el mismo punto (o «Confirmar jugada»; «Cancelar» lo deshace). Visor con árbol horizontal, que solo baja a otra fila cuando una rama chocaría con otra; no entran en el repaso.
- **Josekis** — *Biblioteca* (un archivo por joseki, agrupados en carpetas; importa varios .sgf, un .zip o una carpeta del dispositivo — sus subcarpetas pasan a ser carpetas; cada joseki es un árbol SGF con opciones A, B, C…, pérdida de puntos y comentario por jugada, botón «+ Tenuki» (pasar): crea una rama donde quien juega pasa y sigues con la secuencia local que resulta, «Tenuki» la abre si ya existe, se guarda como `;B[]`/`;W[]` en el SGF; exportar .sgf por archivo; al importar, el título es el nombre del archivo; las carpetas se activan o desactivan desde Ajustes: igual que con los tsumegos, Ajustes → Repaso tiene una lista plegable «Carpetas de josekis a practicar» (con buscador y Marcar/Quitar visibles), la biblioteca puede eliminar una carpeta completa (doble toque) y Ajustes → Datos muestra josekis activos / total; los archivos de carpetas desactivadas siguen en la biblioteca pero no entran en Practicar) y *Practicar* (puedes practicar toda la biblioteca, una carpeta o un archivo; la máquina responde al azar entre tus ramas).
- **Ajustes** — submenú de tres secciones: *Repaso* (límite diario, variantes de tsumegos y josekis, carpetas a estudiar (plegable, con buscador y «Marcar/Quitar visibles»), reiniciar fechas y repasos con confirmación y numeración automática de títulos), *Tablero* (recorte, coordenadas, entrada de jugadas y valores por defecto al crear: tamaño, piedra inicial y primer turno de los tsumegos, y tamaño de las partidas) y *Datos* (almacenamiento con tarjetas activas —con solución y en las carpetas a estudiar— frente al total, respaldo, importar/exportar, copiar y pegar). Las acciones sin cambio visible muestran un aviso breve arriba.

## Jugar en repaso y práctica

Toca un punto para seleccionarlo; doble toque (o **Confirmar jugada**) para jugarlo. En josekis: **Tenuki** pasa: sigue la rama de tenuki de tu biblioteca (si no existe cuenta como fallo), **Ver opciones** muestra las letras, En partidas y josekis las marcas (triángulo, cuadrado, letra y número) usan el mismo selector que el editor de tsumegos y se guardan por posición en el SGF (TR/SQ/LB). Partidas, josekis y el editor de tsumegos muestran las últimas 10 jugadas sobre el tablero (‹ muestra el resto). Tocar fuera del tablero deshace la jugada seleccionada. En partidas y josekis (editor), **Ajustes → Entrada de jugadas** permite elegir entre *Doble toque con zoom* (el primer toque selecciona y acerca el tablero) y *Doble toque sin zoom*; en ambos, un segundo toque en el mismo punto o «Confirmar jugada» la juega. La piedra de selección toma el color de quien juega. El editor de tsumegos coloca la jugada directamente. En josekis puedes marcar jugadas con ★. **Atrás** retrocede 2 posiciones (tu jugada y la respuesta; 1 si juegas con ambos colores) y **Reiniciar** vuelve al inicio.

## Cómo se programa el repaso

Hay solo dos resultados. **Acierto** (llegas al final de la línea sin errores): el primer acierto da 1 día y después el intervalo se multiplica por 1.5 (1, 2, 3, 5, 8, 12… días). **Error** (una jugada fuera del árbol, caer en una línea incorrecta o «Mostrar línea»): la tarjeta vuelve a empezar y se repite hoy. **✎ Editar** no cuenta como acierto ni como error: la tarjeta sigue pendiente y vuelve al repaso al regresar. **Ajustes → Reiniciar fechas y repasos** deja todas las tarjetas como nuevas sin tocar nada más.

**Correcta / incorrecta:** marcar una jugada marca también todas las ramas que parten de ella (y las jugadas nuevas heredan el veredicto). El repaso siempre llega al final de la secuencia; el rival prioriza la línea que lleva a un resultado incorrecto cuando existe.

## Josekis: archivos y carpetas

Cada joseki es un archivo con carpeta opcional. **Dividir aquí** (en el editor) mueve la jugada actual y lo que sigue a un archivo nuevo, conservando la línea que lleva hasta ahí. La miniatura muestra la primera posición donde el árbol se bifurca.

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

Tarjetas, partidas y josekis se guardan solos al editar; no hay botones de guardar (en tarjetas quedan «← Anterior», «Siguiente →» (con el contador «3/25» tocable para saltar) y «Nueva tarjeta»; si entras desde un repaso, «← Volver al repaso»). Los avisos de «guardado» e importación ya no aparecen; solo se muestran los errores (jugada ilegal, archivo ilegible).

## Respaldo

Ajustes → **Respaldo completo** exporta un .json con todo (tarjetas con su calendario, partidas, biblioteca de josekis y ajustes) y lo restaura en otro navegador. Las partidas ya guardadas y la biblioteca se autoguardan al editar.

## Nota sobre los datos

Todo lo que crees (tarjetas, repasos, ajustes) se guarda con IndexedDB, **en ese navegador y en ese dispositivo únicamente**. Al abrir esta versión por primera vez, lo que tenías en `localStorage` se copia a IndexedDB y, una vez verificado, se borra del viejo. **Importar libros de tsumegos** (Ajustes → Datos): acepta varios .sgf y .zip a la vez. Un .sgf con un libro entero (raíz sin piedras y cada rama con su posición AB/AW, como el de Cho Chikun) se separa en una tarjeta por problema («Problema 1…»), en una carpeta con el nombre del libro; un .zip con SGF sueltos usa su nombre y subcarpetas como carpeta. El color que juega (`PL[B]`/`PL[W]`) se guarda en la tarjeta (`toPlay`) y al editar un borrador sin jugadas el editor empieza con ese color. Si el SGF no trae solución, la tarjeta queda como borrador (no entra en repaso hasta que le marques una línea correcta). Los ids son estables: reimportar no duplica. Con bases grandes (decenas de miles de tarjetas) la lista de tarjetas muestra 60 a la vez («Mostrar más») para seguir siendo ágil. No hay servidor ni sincronización — para pasar tus tarjetas a otro navegador o dispositivo, usa las opciones de exportar/importar (.zip, .sgf, o copiar y pegar como texto) que están dentro de la pestaña "Ajustes" de la app.


El veredicto (✓/✗) se aplica a toda la línea: la jugada, todo lo que sigue y las anteriores hacia atrás hasta el punto donde la línea se bifurca (esa jugada no se toca), sobrescribiendo lo que hubiera.
