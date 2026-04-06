# 13.1 Recorte con `<clipPath>`

`<clipPath>` define una región de recorte: solo lo que está **dentro** de esa región es visible. Es un recorte binario — no hay transparencias parciales. Piensa en unas tijeras: o hay papel o no lo hay.

---

## La idea básica

```svg
<svg width="300" height="150"
     viewBox="0 0 300 150"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">recorte binario: dentro o fuera</text>

  <defs>
    <clipPath id="circulo-recorte">
      <circle cx="215" cy="85" r="45"/>
    </clipPath>
  </defs>

  <!-- Sin recortar -->
  <rect x="25" y="40" width="90" height="90" fill="#3b82f6"/>
  <text x="70" y="145" text-anchor="middle" font-size="8" fill="#64748b">sin recorte</text>

  <!-- Con clipPath: solo se ve el rectángulo dentro del círculo -->
  <rect x="170" y="40" width="90" height="90" fill="#3b82f6"
        clip-path="url(#circulo-recorte)"/>
  <!-- Referencia del círculo en trazo para entender qué recorta -->
  <circle cx="215" cy="85" r="45" fill="none" stroke="#ef4444" stroke-width="1" stroke-dasharray="3,2"/>
  <text x="215" y="145" text-anchor="middle" font-size="8" fill="#64748b">clip-path="url(#...)"</text>
</svg>
```

---

## Definición en `<defs>` y aplicación con `clip-path`

```svg
<svg width="300" height="150"
     viewBox="0 0 300 150"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">definir y reutilizar</text>

  <defs>
    <!-- Una estrella como región de recorte -->
    <clipPath id="estrella-clip">
      <polygon points="50,25 60,48 85,48 65,63 72,88 50,73 28,88 35,63 15,48 40,48"/>
    </clipPath>
  </defs>

  <!-- Misma definición aplicada a 3 elementos distintos -->
  <g clip-path="url(#estrella-clip)">
    <rect x="10" y="30" width="80" height="70" fill="#ec4899"/>
  </g>
  <text x="50" y="120" text-anchor="middle" font-size="7" fill="#64748b">aplicado a rect</text>

  <g transform="translate(100, 0)">
    <g clip-path="url(#estrella-clip)">
      <rect x="10" y="30" width="80" height="70" fill="url(#g-arcoiris)"/>
    </g>
  </g>
  <defs>
    <linearGradient id="g-arcoiris" x1="0" y1="0" x2="1" y2="1">
      <stop offset="0" stop-color="#ef4444"/>
      <stop offset="0.5" stop-color="#f59e0b"/>
      <stop offset="1" stop-color="#3b82f6"/>
    </linearGradient>
  </defs>
  <text x="150" y="120" text-anchor="middle" font-size="7" fill="#64748b">aplicado a gradiente</text>

  <g transform="translate(200, 0)">
    <g clip-path="url(#estrella-clip)">
      <circle cx="50" cy="50" r="20" fill="#10b981"/>
      <circle cx="30" cy="80" r="15" fill="#f59e0b"/>
      <circle cx="70" cy="85" r="18" fill="#ec4899"/>
    </g>
  </g>
  <text x="250" y="120" text-anchor="middle" font-size="7" fill="#64748b">aplicado a grupo</text>

  <text x="150" y="140" text-anchor="middle" font-size="8" fill="#94a3b8">
    la misma estrella recorta cualquier contenido
  </text>
</svg>
```

---

## Solo importa la geometría: fill y stroke se ignoran

```svg
<svg width="300" height="160"
     viewBox="0 0 300 160"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">en clipPath, el color no importa</text>

  <defs>
    <!-- Tres clipPaths idénticos en geometría pero con fills diferentes: todos recortan igual -->
    <clipPath id="c1">
      <circle cx="50" cy="60" r="30" fill="red"/>
    </clipPath>
    <clipPath id="c2">
      <circle cx="50" cy="60" r="30" fill="none" stroke="#000" stroke-width="20"/>
    </clipPath>
    <clipPath id="c3">
      <circle cx="50" cy="60" r="30" opacity="0.1"/>
    </clipPath>
  </defs>

  <rect x="10" y="30" width="80" height="60" fill="#3b82f6" clip-path="url(#c1)"/>
  <text x="50" y="105" text-anchor="middle" font-size="7" fill="#64748b">fill="red"</text>

  <rect x="110" y="30" width="80" height="60" fill="#3b82f6" clip-path="url(#c2)"/>
  <text x="150" y="105" text-anchor="middle" font-size="7" fill="#64748b">fill="none" stroke</text>

  <rect x="210" y="30" width="80" height="60" fill="#3b82f6" clip-path="url(#c3)"/>
  <text x="250" y="105" text-anchor="middle" font-size="7" fill="#64748b">opacity="0.1"</text>

  <text x="150" y="130" text-anchor="middle" font-size="8" fill="#10b981">
    los 3 producen el mismo recorte
  </text>
  <text x="150" y="145" text-anchor="middle" font-size="7" fill="#94a3b8">
    solo la geometría cuenta — el estilo se ignora
  </text>
</svg>
```

---

## Formas válidas dentro de `<clipPath>`

```svg
<svg width="300" height="200"
     viewBox="0 0 300 200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">qué puede ir en un clipPath</text>

  <defs>
    <clipPath id="cp-rect"><rect x="10" y="35" width="60" height="45"/></clipPath>
    <clipPath id="cp-ellipse"><ellipse cx="115" cy="58" rx="35" ry="22"/></clipPath>
    <clipPath id="cp-poly"><polygon points="200,35 245,35 260,80 205,80 190,55"/></clipPath>
    <clipPath id="cp-path"><path d="M 15 120 Q 40 100 60 120 T 90 120 L 90 160 L 15 160 Z"/></clipPath>
    <clipPath id="cp-text"><text x="110" y="150" font-size="30" font-weight="bold" font-family="sans-serif">SVG</text></clipPath>
    <clipPath id="cp-group">
      <circle cx="230" cy="125" r="15"/>
      <rect x="215" y="135" width="30" height="20"/>
    </clipPath>

    <linearGradient id="cp-grad" x1="0" y1="0" x2="1" y2="1">
      <stop offset="0" stop-color="#3b82f6"/>
      <stop offset="1" stop-color="#ec4899"/>
    </linearGradient>
  </defs>

  <rect x="0" y="25"  width="300" height="75" fill="url(#cp-grad)" clip-path="url(#cp-rect)"/>
  <text x="40" y="95" text-anchor="middle" font-size="7" fill="#64748b">&lt;rect&gt;</text>

  <rect x="0" y="25" width="300" height="75" fill="url(#cp-grad)" clip-path="url(#cp-ellipse)"/>
  <text x="115" y="95" text-anchor="middle" font-size="7" fill="#64748b">&lt;ellipse&gt;</text>

  <rect x="0" y="25" width="300" height="75" fill="url(#cp-grad)" clip-path="url(#cp-poly)"/>
  <text x="225" y="95" text-anchor="middle" font-size="7" fill="#64748b">&lt;polygon&gt;</text>

  <rect x="0" y="100" width="300" height="65" fill="url(#cp-grad)" clip-path="url(#cp-path)"/>
  <text x="50" y="180" text-anchor="middle" font-size="7" fill="#64748b">&lt;path&gt;</text>

  <rect x="0" y="100" width="300" height="65" fill="url(#cp-grad)" clip-path="url(#cp-text)"/>
  <text x="145" y="180" text-anchor="middle" font-size="7" fill="#64748b">&lt;text&gt;</text>

  <rect x="0" y="100" width="300" height="65" fill="url(#cp-grad)" clip-path="url(#cp-group)"/>
  <text x="230" y="180" text-anchor="middle" font-size="7" fill="#64748b">grupo (unión)</text>
</svg>
```

---

## Unión de varias formas en un mismo clipPath

```svg
<svg width="300" height="160"
     viewBox="0 0 300 160"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">multiples formas = unión</text>

  <defs>
    <clipPath id="cp-burbujas">
      <circle cx="60" cy="80" r="25"/>
      <circle cx="95" cy="60" r="18"/>
      <circle cx="100" cy="95" r="15"/>
      <circle cx="40" cy="55" r="12"/>
    </clipPath>

    <clipPath id="cp-trio">
      <rect x="185" y="55" width="30" height="50"/>
      <rect x="220" y="55" width="30" height="50"/>
      <rect x="255" y="55" width="30" height="50"/>
    </clipPath>

    <linearGradient id="grad-u" x1="0" y1="0" x2="1" y2="0">
      <stop offset="0" stop-color="#f59e0b"/>
      <stop offset="1" stop-color="#ec4899"/>
    </linearGradient>
  </defs>

  <!-- Cluster de burbujas: la región visible es la unión de todos los círculos -->
  <rect x="10" y="35" width="130" height="90" fill="url(#grad-u)" clip-path="url(#cp-burbujas)"/>
  <text x="75" y="140" text-anchor="middle" font-size="7" fill="#64748b">unión de círculos</text>

  <!-- Tres rectángulos separados -->
  <rect x="160" y="35" width="130" height="90" fill="url(#grad-u)" clip-path="url(#cp-trio)"/>
  <text x="225" y="140" text-anchor="middle" font-size="7" fill="#64748b">3 rects separados</text>

  <text x="150" y="155" text-anchor="middle" font-size="8" fill="#94a3b8">
    varias formas en un clipPath = su unión (nunca intersección o resta)
  </text>
</svg>
```

---

## Recortar imagen dentro de una letra

```svg
<svg width="300" height="160"
     viewBox="0 0 300 160"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">&lt;text&gt; como clipPath</text>

  <defs>
    <clipPath id="cp-big-text">
      <text x="150" y="115" font-size="88" font-weight="900"
            text-anchor="middle" font-family="sans-serif">ART</text>
    </clipPath>

    <linearGradient id="grad-art" x1="0" y1="0" x2="1" y2="1">
      <stop offset="0"   stop-color="#8b5cf6"/>
      <stop offset="0.5" stop-color="#ec4899"/>
      <stop offset="1"   stop-color="#f59e0b"/>
    </linearGradient>
  </defs>

  <!-- El fondo es el gradiente recortado por la forma de la palabra "ART" -->
  <rect x="0" y="30" width="300" height="105" fill="url(#grad-art)" clip-path="url(#cp-big-text)"/>

  <text x="150" y="150" text-anchor="middle" font-size="8" fill="#94a3b8">
    técnica clásica: texto grande + recorte sobre gradiente
  </text>
</svg>
```
