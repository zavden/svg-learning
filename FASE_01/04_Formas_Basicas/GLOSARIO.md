# Glosario — Formas Básicas

---

## `fill`

Color de relleno del interior del elemento. Valor por defecto: `black`. Acepta colores CSS, `none` o `url(#id)` para gradientes/patrones.

```svg
<svg width="300" height="70"
     viewBox="0 0 300 70"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="10" y="10" width="50" height="50" fill="#3b82f6"/>
  <rect x="75" y="10" width="50" height="50" fill="none" stroke="#64748b" stroke-width="1.5"/>
  <rect x="140" y="10" width="50" height="50" fill="tomato"/>

  <defs>
    <linearGradient id="g-fill" x1="0" x2="1">
      <stop offset="0%" stop-color="#a855f7"/>
      <stop offset="100%" stop-color="#3b82f6"/>
    </linearGradient>
  </defs>
  <rect x="205" y="10" width="85" height="50" fill="url(#g-fill)" rx="4"/>

  <text x="35"  y="68" text-anchor="middle" font-size="7" fill="#64748b">color</text>
  <text x="100" y="68" text-anchor="middle" font-size="7" fill="#64748b">none</text>
  <text x="165" y="68" text-anchor="middle" font-size="7" fill="#64748b">nombre CSS</text>
  <text x="247" y="68" text-anchor="middle" font-size="7" fill="#64748b">url(#gradiente)</text>
</svg>
```

---

## `stroke`

Color del trazo (borde) del elemento. Sin `stroke`, no hay borde visible. En `<line>`, es **obligatorio** para que sea visible.

```svg
<svg width="300" height="70"
     viewBox="0 0 300 70"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="10" y="10" width="50" height="50" fill="#dbeafe"/>
  <text x="35" y="68" text-anchor="middle" font-size="7" fill="#64748b">sin stroke</text>

  <rect x="80" y="10" width="50" height="50" fill="#dbeafe" stroke="#3b82f6" stroke-width="2"/>
  <text x="105" y="68" text-anchor="middle" font-size="7" fill="#64748b">stroke=blue</text>

  <line x1="160" y1="35" x2="240" y2="35" stroke="#10b981" stroke-width="3"/>
  <text x="200" y="60" text-anchor="middle" font-size="7" fill="#64748b">line necesita stroke</text>
  <line x1="155" y1="25" x2="155" y2="45" stroke="none"/>
  <!-- invisible sin stroke -->
</svg>
```

---

## `stroke-width`

Grosor del trazo en unidades del sistema de coordenadas. El trazo se centra sobre el borde geométrico (mitad adentro, mitad afuera).

```svg
<svg width="300" height="80"
     viewBox="0 0 300 80"
     xmlns="http://www.w3.org/2020/svg"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <line x1="20" y1="25" x2="280" y2="25" stroke="#3b82f6" stroke-width="1"/>
  <text x="150" y="18" text-anchor="middle" font-size="7" fill="#64748b">stroke-width=1</text>

  <line x1="20" y1="50" x2="280" y2="50" stroke="#10b981" stroke-width="5"/>
  <text x="150" y="43" text-anchor="middle" font-size="7" fill="#64748b">stroke-width=5</text>

  <line x1="20" y1="72" x2="280" y2="72" stroke="#f59e0b" stroke-width="12"/>
  <text x="150" y="65" text-anchor="middle" font-size="7" fill="#64748b">stroke-width=12</text>
</svg>
```

---

## `stroke-linecap`

Forma de los extremos de líneas y trazos abiertos. Valores: `butt` (corte recto, defecto), `round` (semicírculo), `square` (cuadrado extendido).

```svg
<svg width="300" height="85"
     viewBox="0 0 300 85"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <line x1="40" y1="22" x2="200" y2="22" stroke="#3b82f6" stroke-width="14" stroke-linecap="butt"/>
  <text x="215" y="26" font-size="8" fill="#64748b">butt</text>

  <line x1="40" y1="50" x2="200" y2="50" stroke="#10b981" stroke-width="14" stroke-linecap="round"/>
  <text x="215" y="54" font-size="8" fill="#64748b">round</text>

  <line x1="40" y1="78" x2="200" y2="78" stroke="#f59e0b" stroke-width="14" stroke-linecap="square"/>
  <text x="215" y="82" font-size="8" fill="#64748b">square</text>

  <!-- Guías de los puntos exactos -->
  <line x1="40" y1="8" x2="40" y2="85" stroke="#ef4444" stroke-width="1" stroke-dasharray="2,2"/>
  <line x1="200" y1="8" x2="200" y2="85" stroke="#ef4444" stroke-width="1" stroke-dasharray="2,2"/>
</svg>
```

---

## `stroke-dasharray`

Define un patrón de guiones y espacios para el trazo. Los valores se alternan: guión, espacio, guión, espacio...

```svg
<svg width="300" height="90"
     viewBox="0 0 300 90"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <line x1="10" y1="20" x2="230" y2="20" stroke="#3b82f6" stroke-width="2" stroke-dasharray="8,4"/>
  <text x="238" y="24" font-size="7.5" fill="#64748b">"8,4"</text>

  <line x1="10" y1="45" x2="230" y2="45" stroke="#10b981" stroke-width="2.5" stroke-dasharray="2,5" stroke-linecap="round"/>
  <text x="238" y="49" font-size="7.5" fill="#64748b">"2,5"</text>

  <line x1="10" y1="70" x2="230" y2="70" stroke="#a855f7" stroke-width="2" stroke-dasharray="12,3,2,3"/>
  <text x="238" y="74" font-size="7.5" fill="#64748b">"12,3,2,3"</text>
</svg>
```

---

## `points` (polyline / polygon)

Lista de pares de coordenadas `x,y` que definen los vértices. Separados por espacios o comas. Formato: `"x1,y1 x2,y2 x3,y3"`.

---

## `fill-rule`

Determina qué partes del interior de un polígono complejo se consideran "dentro". `nonzero` (defecto): todo lo que rodea el contorno está dentro. `evenodd`: alterna interior/exterior en cada cruce.

```svg
<svg width="300" height="100"
     viewBox="0 0 300 100"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Estrella nonzero (todo relleno) -->
  <polygon
    points="75,10 86.76,45.27 123.53,45.27 94.39,66.18 105.71,101.45 75,80.71 44.29,101.45 55.61,66.18 26.47,45.27 63.24,45.27"
    fill="#3b82f6" fill-rule="nonzero"/>
  <text x="75" y="98" text-anchor="middle" font-size="8" fill="#64748b">nonzero</text>

  <!-- Estrella evenodd (centro hueco) -->
  <polygon
    points="225,10 236.76,45.27 273.53,45.27 244.39,66.18 255.71,101.45 225,80.71 194.29,101.45 205.61,66.18 176.47,45.27 213.24,45.27"
    fill="#3b82f6" fill-rule="evenodd"/>
  <text x="225" y="98" text-anchor="middle" font-size="8" fill="#64748b">evenodd</text>
</svg>
```

---

## `rx` / `ry`

En `<rect>`: radio de las esquinas redondeadas (horizontal/vertical). Si solo se especifica `rx`, `ry` toma el mismo valor.
En `<ellipse>`: semiejes horizontal y vertical.

---

## Painter's Model (modelo del pintor)

Los elementos SVG se renderizan **en orden de aparición** en el documento. El último elemento en el código queda encima visualmente. No existe `z-index` en SVG 1.1.
