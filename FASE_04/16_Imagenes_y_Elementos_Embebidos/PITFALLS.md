# Errores comunes — Imágenes y elementos embebidos

---

## 1. `<image>` sin `width` y `height`

El elemento `<image>` no deduce su tamaño automáticamente del archivo referenciado. Si omites las dimensiones, el navegador normalmente **no renderiza nada** o usa valores por defecto impredecibles (en algunos motores 0×0, en otros el tamaño intrínseco pero solo cuando la imagen termina de cargarse, produciendo "saltos" de layout).

```svg
<!-- ✗ mal: sin dimensiones -->
<image href="logo.png" x="20" y="20"/>

<!-- ✓ bien: siempre declara ancho y alto -->
<image href="logo.png" x="20" y="20" width="100" height="100"/>
```

A diferencia de `<img>` de HTML, `<image>` de SVG **requiere** dimensiones explícitas. Pensarlo así: un SVG debe ser renderizable sin esperar a que carguen sus recursos.

---

## 2. `<foreignObject>` sin declarar el namespace XHTML

Dentro de `<foreignObject>`, el navegador necesita saber que el contenido es HTML y no más SVG. Si el `<div>` (o cualquier elemento raíz interno) no declara el namespace `http://www.w3.org/1999/xhtml`, muchos navegadores **simplemente ignoran** todo el contenido, aunque a veces funciona aparentemente — sin garantías.

```svg
<!-- ✗ mal: el navegador puede tratar <div> como SVG desconocido -->
<foreignObject x="10" y="10" width="200" height="100">
  <div>contenido que puede no aparecer</div>
</foreignObject>

<!-- ✓ bien: namespace explícito -->
<foreignObject x="10" y="10" width="200" height="100">
  <div xmlns="http://www.w3.org/1999/xhtml">contenido seguro</div>
</foreignObject>
```

El namespace solo se declara una vez en el elemento raíz del HTML interno; los hijos lo heredan.

---

## 3. Asumir que `<foreignObject>` funciona cuando el SVG se exporta como imagen

Este es el fallo más doloroso: desarrollas un SVG con textos multilínea elegantes usando `<foreignObject>`, todo se ve perfecto en el navegador, y luego alguien:

- lo guarda como `.svg` y lo abre desde el navegador directamente — los textos desaparecen
- lo convierte a PNG con herramientas como Inkscape, rsvg-convert, Puppeteer con ciertas configuraciones — los textos desaparecen
- lo referencia con `<img src="archivo.svg">` — los textos desaparecen
- lo usa como `background-image` en CSS — los textos desaparecen

`<foreignObject>` requiere que el SVG esté **inline en un documento HTML** siendo renderizado por un navegador moderno. En cualquier otro contexto, el contenido HTML no se ejecuta.

**Regla**: si el SVG debe viajar (archivo adjunto, asset estático, export a imagen), no uses `<foreignObject>`. Busca alternativas con `<tspan>` para texto multilínea o pre-renderiza el contenido como bitmap.

---

## 4. Confundir `<g>` con `<svg>` anidado

Un `<g>` y un `<svg>` anidado parecen similares: ambos agrupan elementos. Pero tienen comportamientos muy distintos:

| | `<g>` | `<svg>` anidado |
|---|---|---|
| Crea nuevo viewport | No | Sí |
| Permite `viewBox` propio | No | Sí |
| Recorta contenido que rebasa | No | Sí (por defecto) |
| Acepta `x`, `y` | No (usa `transform`) | Sí |
| Aísla coordenadas | No | Sí |
| Coste de render | Bajo | Ligeramente mayor |

```svg
<!-- Grupo: los elementos usan las coordenadas del padre -->
<g transform="translate(50, 50)">
  <circle cx="0" cy="0" r="10"/>  <!-- termina en (50, 50) -->
</g>

<!-- SVG anidado: coordenadas internas propias gracias al viewBox -->
<svg x="50" y="50" width="100" height="100" viewBox="0 0 10 10">
  <circle cx="5" cy="5" r="5"/>  <!-- centro del viewport interno -->
</svg>
```

Error típico: usar `<svg>` anidado cuando un simple `<g>` bastaba, añadiendo complejidad innecesaria. O al revés: intentar recortar contenido con `<g>` (no recorta) cuando lo que necesitas es un `<svg>` anidado.

---

## 5. Base64 para imágenes grandes o repetidas

Embeber una foto de 500 KB como base64 produce una cadena de ~670 KB dentro del SVG. Si ese SVG se reutiliza en 10 páginas, cada página descarga los 670 KB y **no se cachea** como recurso independiente. Lo mismo si la misma imagen se embebe varias veces en el mismo SVG: cada instancia es una copia literal completa.

```svg
<!-- ✗ mal: misma foto 3 veces = 3 copias completas -->
<image href="data:image/jpeg;base64,/9j/4AAQ...[500KB]..." .../>
<image href="data:image/jpeg;base64,/9j/4AAQ...[500KB]..." .../>
<image href="data:image/jpeg;base64,/9j/4AAQ...[500KB]..." .../>

<!-- ✓ mejor: definir una vez y reusar con <use> o como patrón -->
<defs>
  <image id="photo" href="foto.jpg" width="100" height="100"/>
</defs>
<use href="#photo" x="0" y="0"/>
<use href="#photo" x="120" y="0"/>
<use href="#photo" x="240" y="0"/>
```

**Heurística**:
- Íconos pequeños (< 4 KB): base64 puede valer la pena para evitar peticiones HTTP
- SVGs portables que deben viajar solos: sí, aunque crezcan
- Imágenes grandes o repetidas: no, usa archivos externos
- Contenido dinámico que cambia: no, el cache independiente del archivo es más valioso
