# 13.6 Recorte con CSS

La propiedad CSS `clip-path` permite recortar elementos SVG (y HTML) sin declarar un `<clipPath>` dentro del documento. Soporta formas básicas, paths y referencia a `<clipPath>` SVG. Se puede animar con transiciones CSS.

---

## Funciones de forma básica

```svg
<svg width="300" height="200"
     viewBox="0 0 300 200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">clip-path CSS — formas básicas</text>

  <!-- circle() -->
  <rect x="20" y="40" width="60" height="60" fill="#3b82f6"
        style="clip-path: circle(50%);"/>
  <text x="50" y="115" text-anchor="middle" font-size="7" fill="#64748b">circle(50%)</text>

  <!-- ellipse() -->
  <rect x="95" y="40" width="60" height="60" fill="#10b981"
        style="clip-path: ellipse(50% 35% at 50% 50%);"/>
  <text x="125" y="115" text-anchor="middle" font-size="7" fill="#64748b">ellipse(50% 35%)</text>

  <!-- inset() con esquinas redondeadas -->
  <rect x="170" y="40" width="60" height="60" fill="#ec4899"
        style="clip-path: inset(10% round 15%);"/>
  <text x="200" y="115" text-anchor="middle" font-size="7" fill="#64748b">inset(10% round)</text>

  <!-- polygon() -->
  <rect x="245" y="40" width="50" height="60" fill="#f59e0b"
        style="clip-path: polygon(50% 0, 100% 100%, 0 100%);"/>
  <text x="270" y="115" text-anchor="middle" font-size="7" fill="#64748b">polygon()</text>

  <!-- Ejemplos extra abajo -->
  <rect x="30" y="140" width="90" height="40" fill="#8b5cf6"
        style="clip-path: polygon(0 0, 90% 0, 100% 50%, 90% 100%, 0 100%);"/>
  <text x="75" y="195" text-anchor="middle" font-size="7" fill="#64748b">flecha con polygon</text>

  <rect x="170" y="140" width="110" height="40" fill="#ec4899"
        style="clip-path: polygon(15px 0, calc(100% - 15px) 0, 100% 50%, calc(100% - 15px) 100%, 15px 100%, 0 50%);"/>
  <text x="225" y="195" text-anchor="middle" font-size="7" fill="#64748b">chevron</text>
</svg>
```

---

## `clip-path: url(#id)` — referencia a un `<clipPath>` SVG

```svg
<svg width="300" height="170"
     viewBox="0 0 300 170"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <style>
    .cajita { clip-path: url(#cp-est); }
  </style>

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">CSS + &lt;clipPath&gt; SVG</text>

  <defs>
    <clipPath id="cp-est" clipPathUnits="objectBoundingBox">
      <polygon points="0.5,0 0.62,0.38 1,0.38 0.69,0.62 0.81,1 0.5,0.77 0.19,1 0.31,0.62 0,0.38 0.38,0.38"/>
    </clipPath>

    <linearGradient id="grad-url" x1="0" y1="0" x2="1" y2="1">
      <stop offset="0" stop-color="#f59e0b"/>
      <stop offset="1" stop-color="#ec4899"/>
    </linearGradient>
  </defs>

  <!-- Las 3 rect comparten la misma clase → el mismo clip-path URL -->
  <rect class="cajita" x="30"  y="40" width="70" height="70" fill="url(#grad-url)"/>
  <rect class="cajita" x="115" y="40" width="70" height="70" fill="#3b82f6"/>
  <rect class="cajita" x="200" y="40" width="70" height="70" fill="#10b981"/>

  <text x="65"  y="130" text-anchor="middle" font-size="7" fill="#64748b">gradiente</text>
  <text x="150" y="130" text-anchor="middle" font-size="7" fill="#64748b">azul</text>
  <text x="235" y="130" text-anchor="middle" font-size="7" fill="#64748b">verde</text>

  <text x="150" y="155" text-anchor="middle" font-size="7" fill="#94a3b8">
    .cajita { clip-path: url(#cp-est); }
  </text>
</svg>
```

---

## Animar `clip-path` con CSS transitions

```svg
<svg width="300" height="170"
     viewBox="0 0 300 170"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <style>
    .anim-cajita {
      clip-path: circle(20% at 50% 50%);
      animation: reveal 2.5s ease-in-out infinite alternate;
    }
    @keyframes reveal {
      0%   { clip-path: circle(10% at 50% 50%); }
      100% { clip-path: circle(55% at 50% 50%); }
    }

    .anim-barrido {
      animation: barrido 2.5s ease-in-out infinite alternate;
    }
    @keyframes barrido {
      0%   { clip-path: inset(0 100% 0 0); }
      100% { clip-path: inset(0 0 0 0); }
    }
  </style>

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">clip-path animable con CSS</text>

  <defs>
    <linearGradient id="anim-grad" x1="0" y1="0" x2="1" y2="1">
      <stop offset="0" stop-color="#8b5cf6"/>
      <stop offset="1" stop-color="#ec4899"/>
    </linearGradient>
  </defs>

  <!-- Animación 1: círculo que crece -->
  <rect class="anim-cajita" x="30" y="45" width="90" height="80" fill="url(#anim-grad)"/>
  <text x="75" y="145" text-anchor="middle" font-size="7" fill="#64748b">circle() animado</text>

  <!-- Animación 2: barrido horizontal con inset -->
  <rect class="anim-barrido" x="170" y="55" width="100" height="60" fill="url(#anim-grad)"/>
  <text x="220" y="145" text-anchor="middle" font-size="7" fill="#64748b">inset() animado</text>

  <text x="150" y="163" text-anchor="middle" font-size="7" fill="#94a3b8">
    clip-path en keyframes → transiciones de revelar
  </text>
</svg>
```

---

## Animar entre `polygon()` con el mismo número de puntos

```svg
<svg width="300" height="170"
     viewBox="0 0 300 170"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <style>
    .forma-morph {
      animation: morph 3s ease-in-out infinite alternate;
    }
    @keyframes morph {
      0%   { clip-path: polygon(50% 0%, 100% 50%, 50% 100%, 0% 50%); }
      50%  { clip-path: polygon(50% 10%, 90% 50%, 50% 90%, 10% 50%); }
      100% { clip-path: polygon(20% 20%, 80% 20%, 80% 80%, 20% 80%); }
    }
  </style>

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">morphing de polygon</text>

  <defs>
    <linearGradient id="morph-grad" x1="0" y1="0" x2="1" y2="1">
      <stop offset="0" stop-color="#3b82f6"/>
      <stop offset="1" stop-color="#10b981"/>
    </linearGradient>
  </defs>

  <rect class="forma-morph" x="80" y="35" width="140" height="90" fill="url(#morph-grad)"/>

  <text x="150" y="145" text-anchor="middle" font-size="7" fill="#94a3b8">
    para animar entre polygons hace falta MISMO número de puntos
  </text>
  <text x="150" y="158" text-anchor="middle" font-size="7" fill="#94a3b8">
    los vértices interpolan uno a uno
  </text>
</svg>
```

---

## `clip-path: path()` — forma arbitraria

```svg
<svg width="300" height="170"
     viewBox="0 0 300 170"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <style>
    .wave {
      clip-path: path('M 0 20 Q 50 0 100 20 T 200 20 L 200 100 L 0 100 Z');
    }
  </style>

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">clip-path: path('...')</text>

  <!-- foreignObject con un "div" tipo onda -->
  <rect class="wave" x="50" y="40" width="200" height="100" fill="#ec4899"/>

  <text x="150" y="155" text-anchor="middle" font-size="7" fill="#94a3b8">
    path() acepta sintaxis SVG de path — soporte moderno
  </text>
</svg>
```

---

## `clip-path` en CSS vs `<clipPath>` en SVG

```svg
<svg width="300" height="210"
     viewBox="0 0 300 210"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" fill="#f8fafc" font-weight="bold">
    CSS clip-path vs &lt;clipPath&gt;
  </text>

  <!-- CSS -->
  <rect x="10" y="34" width="135" height="165" fill="#dcfce7" stroke="#10b981" stroke-width="1.5" rx="4"/>
  <text x="77" y="52" text-anchor="middle" font-size="10" font-weight="bold" fill="#166534">CSS clip-path</text>

  <text x="20" y="72"  font-size="7" fill="#1e293b">✓ no modifica el SVG</text>
  <text x="20" y="84"  font-size="7" fill="#1e293b">✓ animable con transitions</text>
  <text x="20" y="96"  font-size="7" fill="#1e293b">✓ funciones nativas</text>
  <text x="20" y="108" font-size="7" fill="#1e293b">  (circle, polygon…)</text>
  <text x="20" y="120" font-size="7" fill="#1e293b">✓ funciona en HTML también</text>
  <text x="20" y="132" font-size="7" fill="#1e293b">✓ puede usar url(#id)</text>
  <text x="20" y="144" font-size="7" fill="#1e293b">✓ unidades CSS (%, px, rem)</text>

  <text x="20" y="164" font-size="7" fill="#ef4444">× sin fill-rule evenodd</text>
  <text x="20" y="176" font-size="7" fill="#ef4444">  (en formas básicas)</text>
  <text x="20" y="188" font-size="7" fill="#ef4444">× path() aún limitado</text>

  <!-- SVG -->
  <rect x="155" y="34" width="135" height="165" fill="#dbeafe" stroke="#3b82f6" stroke-width="1.5" rx="4"/>
  <text x="222" y="52" text-anchor="middle" font-size="10" font-weight="bold" fill="#1e40af">&lt;clipPath&gt; SVG</text>

  <text x="165" y="72"  font-size="7" fill="#1e293b">✓ cualquier forma SVG</text>
  <text x="165" y="84"  font-size="7" fill="#1e293b">✓ path complejo con curvas</text>
  <text x="165" y="96"  font-size="7" fill="#1e293b">✓ texto como recorte</text>
  <text x="165" y="108" font-size="7" fill="#1e293b">✓ fill-rule (evenodd)</text>
  <text x="165" y="120" font-size="7" fill="#1e293b">✓ clipPathUnits</text>
  <text x="165" y="132" font-size="7" fill="#1e293b">✓ soporte universal</text>

  <text x="165" y="152" font-size="7" fill="#ef4444">× el SVG se complica</text>
  <text x="165" y="164" font-size="7" fill="#ef4444">× animar requiere SMIL</text>
  <text x="165" y="176" font-size="7" fill="#ef4444">  o JS (o cambiar la ref.)</text>
  <text x="165" y="188" font-size="7" fill="#ef4444">× no aplica a HTML directo</text>
</svg>
```
