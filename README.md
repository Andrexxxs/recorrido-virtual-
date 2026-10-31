# Terra Volcano · Recorrido virtual 3D

Maqueta de rediseño para [terravolcano.com](https://terravolcano.com) y muestra de recorrido virtual 3D del volcán Villarrica.

| Archivo | Qué es |
| --- | --- |
| `terravolcano-recorrido-3d.html` | Recorrido virtual 3D del Villarrica (MapLibre GL + terreno real + imagen satelital), con 6 paradas y cámara que reproduce una toma de Google Earth. |
| `terravolcano-awwwards.html` | Maqueta del sitio con patrones front-end de Awwwards (Stimulus, GSAP, Lenis, Swiper). |
| `PROMPT-recrear-terravolcano.md` | Prompt para recrear y continuar el proyecto en otra sesión de Claude Code, con las mejoras pendientes. |
| `index.html` | Redirige al recorrido 3D. |

## Ver en local

```bash
python -m http.server 8080
```

Abre `http://localhost:8080/`. Se usa un servidor porque el recorrido carga módulos ES y teselas remotas.

## Fuentes de datos y atribución

- Imagen satelital: Esri World Imagery (Esri, Maxar, Earthstar Geographics). El uso comercial requiere cuenta de ArcGIS.
- Terreno: AWS Terrain Tiles (Mapzen, SRTM y otras fuentes).
- Fotos y datos de tours: terravolcano.com y TourTask, enlazados desde sus servidores.

Para igualar la nitidez de Google Earth hay que usar Google Photorealistic 3D Tiles (Map Tiles API), con una API key guardada en `config.local.js`. Ese archivo está en `.gitignore`. Ver las mejoras pendientes en el prompt.
