# Prompt para recrear el proyecto Terra Volcano en otra sesión

Copia todo lo que está debajo de la línea y pégalo en una sesión nueva de Claude Code abierta en `C:\Users\Andres Martinez\Desktop\Paginas Web`.

---

## Contexto

Necesito recrear, y luego mejorar, dos páginas de concepto para **Terra Volcano** (https://terravolcano.com), una agencia de expediciones a volcanes en Pucón, Chile. Ambas son HTML estático de un solo archivo, sin build y con librerías desde CDN. Las sirvo con `python -m http.server 8080 --bind 127.0.0.1`; la configuración ya existe en `.claude/launch.json` con el nombre `paginas-web-static`.

Si existen, lee primero las memorias del proyecto: `awwwards-frontend-stack`, `awwwards-agency-trends`, `terravolcano-site`, `villarrica-virtual-tour`, `gsap-stimulus-lenis-gotchas` y `rea-web-inspection-workflow`. Si los archivos `terravolcano-awwwards.html` y `terravolcano-recorrido-3d.html` ya están en la carpeta, trabaja sobre ellos en vez de reescribirlos.

## Datos de la marca y del negocio (reales, revisados en octubre de 2026)

- **Sitio actual:** WordPress con tema Sydney, Elementor 4.3.5, addons HT Mega, jQuery y Swiper v8, en hosting SiteGround. SiteGround tiene un reto anti-bot (`/.well-known/sgcaptcha/`) que bloquea el hotlink de imágenes en algunos contextos. Los tours se muestran con el widget TourTask (`tourtask-booking.pages.dev`), con selector de pasajeros y moneda CLP.
- **Marca:**
  - Colores: rojo `#C80000`, oscuro volcánico `#1D0404`, blanco.
  - Tipografías del sitio actual: Montserrat y Roboto. El concepto usa **Inter Tight**, la tipografía de Awwwards.
  - Logo: `https://terravolcano.com/wp-content/uploads/2026/08/terra-volcano-191x50-1.png`. Puede no cargar por el reto de SiteGround; usa un texto "TERRA VOLCANO" como respaldo.
- **Lema:** "Extraordinary expeditions. Exclusively yours." / "Expediciones extraordinarias. Exclusivamente tuyas."
- **Prueba social:** Nº1 en Tripadvisor. Reseñas de Carlos B, flyer13955555012 y Elias Francisco V. Están traducidas al español y deben marcarse como "traducida".
- **Fotos del sitio:** en `https://terravolcano.com/wp-content/uploads/2026/09/`:
  - `climbers-villarica-volcano-summit-overlay-40-1920x1080-1.jpg`
  - `climber-female-tourist-villarica-overlay-40-1920x1280-1.jpg`
  - `skiiers-villarica-volcano-ski-touring-tourists-1672x941-overlay-40.jpg`
  - `tourist-happy-sliding-snow-volcano-overlay-40-1920x1080-1.jpg`
  - `climbers-snow-mountain-flag-chile-800x800-1.jpg`
  - `rated-no-1-tripadvisor-800x800-1.jpg`
- **Fotos de tours:** `https://media.tourtask.com/uploads/tv/tours/tour-{id}-main-750.jpg`.
- **Catálogo:** 28 tours, precios en CLP por persona (G = grupal, P = privado).
  - **Ascensos:** Villarrica 1660 (8 h, G 150.000) · Lanín 1658 (2 d, G 320.000) · Quetrupillán 1661 (11 h, G 170.000) · Llaima 1659 (13 h, G 220.000 / P 290.000) · Mocho-Choshuenco 1663 (1 d, P 400.000) · Sollipulli 1668 (19 h, G 180.000 / P 210.000) · Lonquimay 1764 (15 h, G 180.000).
  - **Backcountry:** Villarrica 1803 (10 h–1 d, G 240.000 / P 300.000) · Quetrupillán 1805 (10 h, G 270.000) · Lanín 1806 (1 d, P 490.000) · Llaima 1809 (13 h, P 390.000) · Mocho-Choshuenco 1810 (1 d, P 500.000) · Sollipulli 1811 (19 h, P 280.000) · Ski touring Lonquimay 1812 (15 h, G 390.000).
  - **Trekking:** Lagos Andinos con raquetas 1662 (8 h, P 100.000) · Huerquehue 1667 (9 h, P 100.000) · Santuario El Cañi 1769 (10 h, P 100.000).
  - **Invierno:** clases grupales 2 h 1705 (G 110.000) · privada 2 h + equipo 1728 (P 160.000) · privada 1 h 1706 (P 97.500) · Nieve sin esquís 1730 (G 50.000). Todas duran 9 h.
  - **Agua y aventura:** Canyoning El Claro 1666 (4 h, G 60.000) · Rafting Alto Trancura 1664 (3 h, G 52.000) · Rafting Bajo Trancura 1665 (3 h, G 42.000) · Huilo-Huilo 1669 (12 h, G 50.000).
  - **Travesías:** Villarrica 3 días/2 noches 2450 (G 490.000) · Villarrica 5 días/4 noches 2453 (precio a consultar).
  - **Paquetes:** Trilogía Araucana 1836 (no reservable online).
- **Destino de los botones de reserva:** `https://terravolcano.com/tours/`.

## Página 1: `terravolcano-awwwards.html`

Es un rediseño que aplica los patrones front-end que se obtuvieron con ingeniería inversa de awwwards.com.

**Arquitectura de Awwwards a imitar:**
- HTML renderizado más controladores **Stimulus 3.2.2** (ESM: `https://cdn.jsdelivr.net/npm/@hotwired/stimulus@3.2.2/+esm`), conectados con `data-controller`, `data-action` y `data-*-target/value/param`.
- **GSAP 3.13.0** desde cdnjs, con ScrollTrigger, Flip y SplitText. Desde 3.13 todos los plugins son gratuitos.
- **Lenis 1.3.4** sincronizado con el ticker de GSAP.
- **Swiper 11.1.14**.

**Tokens CSS** (misma nomenclatura que Awwwards):
- Tipografía y espaciado: `--font-1`, `--gutter: 20px`, `--header-height: 71px`.
- Botones: `--button-height: 48px`, `--button-pad: 24px`, `--button-rounded: 8px`, `--button-rounded-full: 72px`, `--rounded-small/normal/large`.
- Imagen: `--img-resizing-site: 16/12`.
- Curva de animación característica: `cubic-bezier(0,1,.5,1)` de 0.6 s.
- Un color por categoría.

**Controladores Stimulus.** Se portaron del código real de Awwwards; hay que mantener el comportamiento:
- **header-marquee:** clona el ítem 10 veces y anima con un timeline de GSAP con `repeat:-1` y `x:-width`. Al pasar el mouse, `gsap.to(tl,{timeScale:0,duration:.5,ease:"power1.inOut"})`. Se pausa cuando está fuera de pantalla.
- **marquee:** clona `clonedTimes` veces y anima con `xPercent:-100` y `repeat:-1`. Mejora añadida: la velocidad y la dirección siguen la velocidad del scroll. Hay que `totalTime(duration*100)` para poder reproducir en reversa y matar el timeline de velocidad anterior.
- **grid:** dropdowns `.filter-item.is-open` con cierre por `click@window` y Esc. Cambio de columnas con `data-cols` 2/3/4 (`--minthumb` 594/444/330 px), animado con Flip y guardado en `localStorage`.
- **tours:** filtra por categoría, modalidad, guardados y búsqueda sin acentos. Ordena por destacados, precio ascendente o descendente y duración. Pagina de 8 en 8. Anima con `Flip.getState/Flip.from` y reutiliza los nodos `<li>` desde un caché. Hace pulso `aniCountPulse` en el contador.
- **infinite-scroll:** IntersectionObserver con `rootMargin "200px"`, guarda `isLoading`. Se comunica con eventos `infinite-scroll:load` y `tours:rendered`.
- **lazyload-image:** pasa `data-src` a `src` y aplica las clases `lazy--hidden` y `lazy--loaded` (opacidad 0.6 s).
- **collectable:** favoritos en `localStorage` (`tv:saved`), rebote elástico al marcar y snackbar de confirmación.
- **Otros:** saved-count, snackbar, floating-header (barra CTA inferior), search (header con estado `is-scrolled`, menú móvil y búsqueda en vivo).
- **hero:**
  - intro con clip-path de `inset(10% 8% … round 28px)` a 0, más SplitText de caracteres con máscara de palabras;
  - parallax de imagen con `yPercent 8` y contenido que se desvanece con ScrollTrigger scrub;
  - brasas en canvas: unas 80 partículas, `globalCompositeOperation lighter`, `mix-blend-mode screen`, viento que sigue al puntero y pausa cuando no se ven.
- **horizontal:** sección "Un día en el volcán" pineada con scroll horizontal (`gsap.matchMedia`, solo con ancho ≥ 900 px), parallax interno con `containerAnimation` y barra de progreso.
- **split-reveal:** divide en líneas con máscara, anima con IntersectionObserver ("appear") y revierte el split al terminar.
- **Resto:** counter (también con appear), swiper (configuraciones por nombre con breakpoints, como Awwwards), clip-reveal (CTA con clip-path atado al scroll).

**Secciones, en orden:** marquee superior → header sticky → hero → marquee tipográfico con nombres de volcanes en contorno → listado de tours (barra de filtros sticky y grid de tarjetas `figure-rollover` con overlay de degradado en hover, botón central "Reservar", insignias de categoría y precio "desde") → "Un día en el volcán" (5 pasos pineados, con botón al recorrido 3D) → cifras (Nº1, 7 volcanes, 28 experiencias, 3.776 m) → opiniones en Swiper → CTA con destello `budgetShiny` → footer con wordmark gigante.

**Problemas conocidos que ya están resueltos (no repetirlos):**
- Cada `ScrollTrigger.refresh()` saca y vuelve a meter el elemento pineado en su `pin-spacer`. Stimulus lo detecta y reconecta los controladores que hay dentro. Por eso las conexiones asíncronas deben usar un token, y los reveals de una sola vez deben marcarse con `data-revealed`.
- **No usar `pinnedContainer`:** colgó el renderer junto con `gsap.matchMedia` y `ScrollTrigger.sort()`.
- No usar `autoSplit` de SplitText con un tween ScrollTrigger devuelto desde `onSplit`: duplica los triggers.
- Registrar los controladores en orden de página y, en `load`, ejecutar `ScrollTrigger.sort()` y luego `refresh()`.
- Todo acceso a `localStorage` va dentro de try/catch.
- El listener global de error de imágenes (que aplica el respaldo `.js-media.is-fallback` con un degradado volcánico) va en un script del `<head>`.
- Respetar `prefers-reduced-motion`: sin Lenis, sin split y con contadores en su valor final.

## Página 2: `terravolcano-recorrido-3d.html`

Es un recorrido virtual 3D del volcán Villarrica que imita la toma y la interfaz de Google Earth.

**Lo que se averiguó de Google Earth Web con REA:**
- Está hecho en Flutter Web con CanvasKit, más un motor propio en WASM. No tiene DOM reutilizable.
- Su imagen y su malla 3D no se pueden copiar.
- Formato de la cámara en la URL: `@lat,lng,{a}a,{d}d,{fov}y,{h}h,{t}t,{r}r`, donde `a` es la altitud del punto mirado y `d` la distancia de la cámara.
- **Toma de referencia:** `@-39.40736408,-71.94651589,2090.58887562a,3876.70974446d,35y,138.35469832h,79.91987491t`, con la cámara a unos 2.770 m.

**Stack:**
- **MapLibre GL 5.6.0** con `maxPitch: 85` y `sky` con niebla.
- **Imagen:** raster de Esri World Imagery, `https://server.arcgisonline.com/ArcGIS/rest/services/World_Imagery/MapServer/tile/{z}/{y}/{x}`, `maxzoom` 18. Atribución obligatoria: "Esri, Maxar, Earthstar Geographics".
- **Relieve:** `raster-dem` de AWS Terrain Tiles en formato terrarium, `https://s3.amazonaws.com/elevation-tiles-prod/terrarium/{z}/{x}/{y}.png`, `maxzoom` 15, con `exaggeration` 1 y sombreado suave.
- Stimulus y GSAP para la interfaz.

**Conversión de cámara de Google Earth a MapLibre:**
- Desplazar la cámara detrás del punto mirado una distancia horizontal `d·sin(t)` en el rumbo `heading+180`.
- Altura de la cámara: `a + d·cos(t)`.
- Luego `map.calculateCameraOptionsFromTo(cameraLngLat, altCam, targetLngLat, a)` y `flyTo`.

**Paradas:**
- Cumbre de referencia: −39.4206, −71.9392, a 2.847 m.
- Paradas: Pucón y el lago (mirando al NO, rumbo 330) · Ladera norte (la toma exacta de Google) · Ascenso · Cráter (con órbita `rotateTo` de +120° en 14 s) · Quetrupillán (2.360 m) y Lanín (3.776 m) · Regreso a Pucón.

**Interfaz:**
- Tarjeta de la parada con animación GSAP.
- Línea de paradas con barra de progreso (variable CSS `--progress`).
- Reproducción automática que se pausa con cualquier gesto, con atajos de teclado ← → y espacio.
- Controles al estilo Google Earth: brújula, 2D/3D, + y −.
- Barra de estado con escala, altura de cámara, coordenadas en grados-minutos-segundos y elevación.
- Marcadores HTML para los puntos de interés.

**Problemas conocidos:**
- Usar `style.load`, no `load`, para colocar la cámara, porque `load` espera a todas las teselas. Ocultar el loader en `idle`, con un tiempo máximo de 9 s.
- `transform.getCameraAltitude()` es relativa al punto mirado; para la altura sobre el mar hay que sumarle `transform.elevation`.
- Las pestañas en segundo plano del panel del navegador frenan `requestAnimationFrame`, así que el mapa debe probarse en una pestaña al frente.

## Mejoras pendientes (hazlas en este orden y verifica cada una en el navegador)

1. **Nitidez como Google Earth:** integrar **Google Photorealistic 3D Tiles** (Map Tiles API) con **CesiumJS**, usando la misma lista de paradas y la misma conversión de cámara. Cesium usa heading/pitch/range: `HeadingPitchRange(h, t-90, d)` sobre el punto mirado.
   - La API key se lee de un `config.local.js`, que debe ir en `.gitignore`; nunca va escrita en el código.
   - Mostrar el logo y la atribución de Google como exigen sus términos.
   - Si no hay key, el recorrido sigue con MapLibre.
   - Necesito crear la key con facturación en Google Cloud; pídemela cuando llegue el momento.
2. **Licencias de imagen:** confirmar la licencia de Esri para uso comercial (cuenta de ArcGIS) o cambiar a otra fuente con licencia clara (MapTiler Satellite con key, o Sentinel-2 cloudless de EOX con su atribución, que tiene menor resolución).
3. **Relieve más fino:** probar un DEM de mayor resolución (por ejemplo, MapTiler Terrain RGB o el DEM de 12,5 m de ALOS PALSAR procesado a teselas) en vez del de 30 m de AWS.
4. **Ruta real de ascenso:** dibujar el track GPS real de la subida (un GeoJSON que me pidas o que se trace en la sesión) como línea 3D sobre el terreno, con un marcador animado que avance durante la parada "Ascenso" y un perfil de elevación.
5. **Puntos de interés verificados:** añadir los miradores que muestra Google Earth (Mirador Columpio frente al Volcán, Mirador Challupén, Glaciar Pichillancahue, Mirador laguna azul) con coordenadas confirmadas, sin inventarlas, y la base del centro de esquí Pillán.
6. **Imágenes propias:** descargar a `assets/` las fotos de los tours y del sitio (en formato WebP con `srcset`) para no depender del hotlink a SiteGround y TourTask. También el logo en SVG o PNG local.
7. **Enlaces de reserva por tour:** cada tarjeta y cada parada debe llevar a la ficha concreta del tour en TourTask, no a la página general `/tours/`.
8. **Idioma:** la web real está en inglés. Añadir un selector ES/EN (diccionario JS o `data-i18n`) en ambas páginas.
9. **Rendimiento y accesibilidad:**
   - Precargar la imagen del hero y la tipografía; usar `fetchpriority`.
   - Auditar con Lighthouse en móvil.
   - Revisar el foco con teclado en las tarjetas y en el recorrido, el contraste del texto sobre las fotos y `aria-live` en los cambios de parada.
   - Bajar las partículas del hero cuando hay batería baja o `saveData`.
10. **Llevarlo a WordPress/Elementor:** plan para portar el concepto al sitio actual (tema hijo o plantilla HTML personalizada, con los scripts encolados) o recomendar una migración a Astro. En las webs de Awwwards, Astro fue el framework más usado.

**Al terminar cada mejora:**
- Probar en escritorio y en móvil, revisar la consola y confirmar que no hay desborde horizontal.
- Actualizar las memorias del proyecto con lo aprendido.
