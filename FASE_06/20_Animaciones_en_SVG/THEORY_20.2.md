# 20.2 Animación SMIL con `<animate>`

`<animate>` es el elemento básico de SMIL: se coloca **como hijo** del elemento que quieres animar y declara qué atributo cambia, desde dónde, hasta dónde y en cuánto tiempo. Todo declarativo, sin una línea de JS o CSS.

---

## Sintaxis básica

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    &lt;animate&gt; como hijo del elemento
  </text>

  <rect x="15" y="38" width="270" height="170" fill="#0f172a" rx="3"/>
  <text x="25" y="56" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;circle cx="50" cy="50" r="20" fill="steelblue"&gt;</text>
  <text x="25" y="72" font-size="8" font-family="monospace" fill="#60a5fa">  &lt;animate</text>
  <text x="25" y="86" font-size="8" font-family="monospace" fill="#fbbf24">    attributeName="cx"</text>
  <text x="25" y="100" font-size="8" font-family="monospace" fill="#34d399">    from="10"</text>
  <text x="25" y="114" font-size="8" font-family="monospace" fill="#34d399">    to="90"</text>
  <text x="25" y="128" font-size="8" font-family="monospace" fill="#34d399">    dur="2s"</text>
  <text x="25" y="142" font-size="8" font-family="monospace" fill="#ec4899">    repeatCount="indefinite"</text>
  <text x="25" y="156" font-size="8" font-family="monospace" fill="#60a5fa">  /&gt;</text>
  <text x="25" y="170" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;/circle&gt;</text>

  <text x="25" y="194" font-size="7" fill="#94a3b8">/* el círculo se mueve de cx=10 a cx=90 en 2 segundos, infinito */</text>
</svg>
```

---

## Ejemplo en vivo

```svg
<svg width="300" height="160"
     viewBox="0 0 300 160"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    SMIL en acción
  </text>

  <line x1="30" y1="90" x2="270" y2="90" stroke="#cbd5e1" stroke-dasharray="3 3"/>

  <circle cx="30" cy="90" r="14" fill="#3b82f6">
    <animate attributeName="cx" from="30" to="270" dur="3s" repeatCount="indefinite"/>
    <animate attributeName="fill" values="#3b82f6;#10b981;#f59e0b;#ec4899;#3b82f6" dur="3s" repeatCount="indefinite"/>
  </circle>

  <text x="150" y="130" text-anchor="middle" font-size="8" fill="#94a3b8">
    se anima cx Y fill simultáneamente, cada uno en su &lt;animate&gt;
  </text>
  <text x="150" y="144" text-anchor="middle" font-size="7" fill="#94a3b8">
    ningún CSS, ningún JavaScript
  </text>
</svg>
```

---

## `attributeName` y `from`/`to`/`values`

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    qué animar y entre qué valores
  </text>

  <rect x="15" y="38" width="270" height="98" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">/* from / to: dos valores */</text>
  <text x="25" y="68" font-size="8" font-family="monospace" fill="#60a5fa">&lt;animate</text>
  <text x="25" y="82" font-size="8" font-family="monospace" fill="#fbbf24">  attributeName="cx"</text>
  <text x="25" y="96" font-size="8" font-family="monospace" fill="#34d399">  from="10" to="90"</text>
  <text x="25" y="110" font-size="8" font-family="monospace" fill="#34d399">  dur="2s"/&gt;</text>
  <text x="25" y="126" font-size="7" fill="#94a3b8">si omites from: comienza desde el valor actual</text>

  <rect x="15" y="144" width="270" height="124" fill="#0f172a" rx="3"/>
  <text x="25" y="160" font-size="7" font-family="monospace" fill="#94a3b8">/* values: secuencia (más expresivo) */</text>
  <text x="25" y="174" font-size="8" font-family="monospace" fill="#60a5fa">&lt;animate</text>
  <text x="25" y="188" font-size="8" font-family="monospace" fill="#fbbf24">  attributeName="cx"</text>
  <text x="25" y="202" font-size="8" font-family="monospace" fill="#34d399">  values="10; 50; 90; 50; 10"</text>
  <text x="25" y="216" font-size="8" font-family="monospace" fill="#34d399">  dur="4s"/&gt;</text>
  <text x="25" y="232" font-size="7" fill="#94a3b8">recorre los 5 valores en orden, distribuidos uniformemente</text>
  <text x="25" y="246" font-size="7" fill="#94a3b8">si usas values, from/to se ignoran</text>
  <text x="25" y="260" font-size="7" fill="#fbbf24">cualquier atributo animable: cx, cy, r, fill, opacity, d…</text>
</svg>
```

---

## Tiempo: `dur`, `begin`, `end`, `repeatCount`

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    control temporal
  </text>

  <rect x="15" y="38" width="270" height="80" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">/* dur — duración de un ciclo */</text>
  <text x="25" y="68" font-size="8" font-family="monospace" fill="#34d399">dur="2s"</text>
  <text x="25" y="82" font-size="8" font-family="monospace" fill="#34d399">dur="500ms"</text>
  <text x="25" y="96" font-size="8" font-family="monospace" fill="#34d399">dur="00:02"</text>
  <text x="25" y="110" font-size="8" font-family="monospace" fill="#34d399">dur="indefinite"</text>

  <rect x="15" y="126" width="270" height="86" fill="#0f172a" rx="3"/>
  <text x="25" y="142" font-size="7" font-family="monospace" fill="#94a3b8">/* begin — cuándo arranca */</text>
  <text x="25" y="156" font-size="8" font-family="monospace" fill="#34d399">begin="0s"             /* defecto */</text>
  <text x="25" y="170" font-size="8" font-family="monospace" fill="#34d399">begin="2s"             /* tras 2 segundos */</text>
  <text x="25" y="184" font-size="8" font-family="monospace" fill="#34d399">begin="click"          /* al click */</text>
  <text x="25" y="198" font-size="8" font-family="monospace" fill="#34d399">begin="otro.end + 1s"  /* encadenada */</text>

  <rect x="15" y="220" width="270" height="50" fill="#0f172a" rx="3"/>
  <text x="25" y="236" font-size="7" font-family="monospace" fill="#94a3b8">/* repeatCount — cuántas veces */</text>
  <text x="25" y="250" font-size="8" font-family="monospace" fill="#34d399">repeatCount="3"</text>
  <text x="25" y="262" font-size="8" font-family="monospace" fill="#34d399">repeatCount="indefinite"</text>
</svg>
```

---

## `fill="freeze"` vs `fill="remove"`

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    qué pasa al terminar
  </text>

  <rect x="15" y="38" width="270" height="80" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#92400e">fill="remove" (defecto)</text>
  <text x="25" y="70" font-size="7" font-family="monospace" fill="#475569">&lt;animate ... fill="remove"/&gt;</text>
  <text x="25" y="84" font-size="7" fill="#475569">al terminar, el atributo vuelve al valor original</text>
  <text x="25" y="98" font-size="7" fill="#475569">útil para animaciones que se repiten o son temporales</text>
  <text x="25" y="110" font-size="7" fill="#92400e">¡cuidado!: una animación de "fade out" parece fallar</text>

  <rect x="15" y="126" width="270" height="100" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="142" font-size="9" font-weight="bold" fill="#166534">fill="freeze"</text>
  <text x="25" y="158" font-size="7" font-family="monospace" fill="#475569">&lt;animate ... fill="freeze"/&gt;</text>
  <text x="25" y="172" font-size="7" fill="#475569">al terminar, el atributo se queda en el valor final</text>
  <text x="25" y="186" font-size="7" fill="#475569">imprescindible para animaciones de "estado final"</text>
  <text x="25" y="200" font-size="7" fill="#166534">→ ej. una transición de entrada que debe quedarse visible</text>
  <text x="25" y="216" font-size="7" fill="#94a3b8">no confundir con fill (color de relleno) en SVG</text>
</svg>
```

---

## `keyTimes`, `keySplines`, `calcMode`

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    timing fino entre valores
  </text>

  <rect x="15" y="38" width="270" height="92" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">/* keyTimes: tiempos normalizados (0..1) */</text>
  <text x="25" y="68" font-size="8" font-family="monospace" fill="#60a5fa">&lt;animate</text>
  <text x="25" y="82" font-size="8" font-family="monospace" fill="#34d399">  values="0; 50; 100"</text>
  <text x="25" y="96" font-size="8" font-family="monospace" fill="#34d399">  keyTimes="0; 0.8; 1"</text>
  <text x="25" y="110" font-size="8" font-family="monospace" fill="#34d399">  dur="5s"/&gt;</text>
  <text x="25" y="124" font-size="7" fill="#94a3b8">80% del tiempo en 0→50, 20% restante en 50→100</text>

  <rect x="15" y="138" width="270" height="60" fill="#0f172a" rx="3"/>
  <text x="25" y="154" font-size="7" font-family="monospace" fill="#94a3b8">/* calcMode: modo de interpolación */</text>
  <text x="25" y="168" font-size="8" font-family="monospace" fill="#34d399">calcMode="linear"    /* defecto */</text>
  <text x="25" y="180" font-size="8" font-family="monospace" fill="#34d399">calcMode="discrete"  /* saltos */</text>
  <text x="25" y="192" font-size="8" font-family="monospace" fill="#34d399">calcMode="paced"     /* velocidad constante */</text>

  <rect x="15" y="206" width="270" height="64" fill="#0f172a" rx="3"/>
  <text x="25" y="222" font-size="7" font-family="monospace" fill="#94a3b8">/* keySplines: curvas Bézier por tramo */</text>
  <text x="25" y="236" font-size="8" font-family="monospace" fill="#34d399">calcMode="spline"</text>
  <text x="25" y="250" font-size="8" font-family="monospace" fill="#34d399">keySplines="0.25 0.1 0.25 1"</text>
  <text x="25" y="262" font-size="7" fill="#94a3b8">→ equivale al ease() de CSS</text>
</svg>
```
