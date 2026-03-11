# Glosario visual — Sección 01: Fundamentos y Conceptos Previos

Cada término con ejemplos SVG que ilustran su significado.

---

## SVG (Scalable Vector Graphics)

Formato de imagen vectorial basado en XML. Describe gráficos como instrucciones matemáticas, no como píxeles.

```svg
<svg width="260" height="80"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block;">
  <!-- Scalable: este mismo SVG se ve nítido a cualquier tamaño -->
  <circle cx="40" cy="40" r="28" fill="#3b82f6"/>
  <text x="80" y="35" font-size="12" fill="#1e293b" font-family="sans-serif" font-weight="700">
    Scalable
  </text>
  <text x="80" y="53" font-size="12" fill="#3b82f6" font-family="sans-serif">
    Vector Graphics
  </text>
</svg>
```

---

## XML (eXtensible Markup Language)

Lenguaje de marcado basado en etiquetas y atributos. SVG usa XML como sintaxis. La regla fundamental: todo debe estar bien formado.

```svg
<svg width="260" height="90"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#0f172a;display:block; font-family:monospace;">

  <!-- XML bien formado: etiqueta, atributo, valor, cierre -->
  <text x="16" y="28" font-size="9" fill="#e06c75">&lt;rect</text>
  <text x="52" y="28" font-size="9" fill="#d19a66">x="10"</text>
  <text x="98" y="28" font-size="9" fill="#d19a66">width="80"</text>
  <text x="176" y="28" font-size="9" fill="#98c379">fill="blue"</text>
  <text x="240" y="28" font-size="9" fill="#56b6c2">/&gt;</text>

  <!-- Anotaciones -->
  <text x="28"  y="52" text-anchor="middle" font-size="7" fill="#475569">etiqueta</text>
  <text x="76"  y="52" text-anchor="middle" font-size="7" fill="#475569">atributo</text>
  <text x="140" y="52" text-anchor="middle" font-size="7" fill="#475569">valor</text>
  <text x="220" y="52" text-anchor="middle" font-size="7" fill="#475569">auto-cierre</text>

  <!-- Resultado visual -->
  <rect x="60" y="62" width="140" height="20" rx="3" fill="#3b82f6"/>
  <text x="130" y="76" text-anchor="middle" font-size="7" fill="white">resultado</text>
</svg>
```

---

## Formato vectorial

Representa gráficos como ecuaciones y coordenadas geométricas, no como píxeles. Resultado: independiente de la resolución.

```svg
<svg width="260" height="100"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Lo que el browser "guarda" internamente -->
  <text x="130" y="20" text-anchor="middle" font-size="9" fill="#64748b">
    Lo que SVG almacena:
  </text>
  <text x="130" y="38" text-anchor="middle" font-size="9" fill="#3b82f6" font-family="monospace">
    cx="130" cy="65" r="28"
  </text>

  <!-- Lo que el browser renderiza -->
  <text x="130" y="55" text-anchor="middle" font-size="9" fill="#64748b">
    Lo que el browser dibuja:
  </text>
  <circle cx="130" cy="80" r="18" fill="#3b82f6"/>
</svg>
```

---

## Formato raster (bitmap)

Almacena una cuadrícula fija de píxeles. Escalar por encima de la resolución original produce pixelación.

```svg
<svg width="260" height="100"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <text x="130" y="18" text-anchor="middle" font-size="9" fill="#64748b">
    Un círculo PNG — vista interna (cuadrícula de píxeles):
  </text>

  <!-- Cuadrícula simplificada de píxeles -->
  <g transform="translate(70, 28)">
    <!-- fila 0 -->
    <rect x="0"  y="0"  width="12" height="12" fill="#f1f5f9"/>
    <rect x="12" y="0"  width="12" height="12" fill="#60a5fa"/>
    <rect x="24" y="0"  width="12" height="12" fill="#3b82f6"/>
    <rect x="36" y="0"  width="12" height="12" fill="#60a5fa"/>
    <rect x="48" y="0"  width="12" height="12" fill="#f1f5f9"/>
    <!-- fila 1 -->
    <rect x="0"  y="12" width="12" height="12" fill="#60a5fa"/>
    <rect x="12" y="12" width="12" height="12" fill="#1d4ed8"/>
    <rect x="24" y="12" width="12" height="12" fill="#1d4ed8"/>
    <rect x="36" y="12" width="12" height="12" fill="#1d4ed8"/>
    <rect x="48" y="12" width="12" height="12" fill="#60a5fa"/>
    <!-- fila 2 -->
    <rect x="0"  y="24" width="12" height="12" fill="#3b82f6"/>
    <rect x="12" y="24" width="12" height="12" fill="#1d4ed8"/>
    <rect x="24" y="24" width="12" height="12" fill="#1e3a8a"/>
    <rect x="36" y="24" width="12" height="12" fill="#1d4ed8"/>
    <rect x="48" y="24" width="12" height="12" fill="#3b82f6"/>
    <!-- fila 3 -->
    <rect x="0"  y="36" width="12" height="12" fill="#60a5fa"/>
    <rect x="12" y="36" width="12" height="12" fill="#1d4ed8"/>
    <rect x="24" y="36" width="12" height="12" fill="#1d4ed8"/>
    <rect x="36" y="36" width="12" height="12" fill="#1d4ed8"/>
    <rect x="48" y="36" width="12" height="12" fill="#60a5fa"/>
    <!-- fila 4 -->
    <rect x="0"  y="48" width="12" height="12" fill="#f1f5f9"/>
    <rect x="12" y="48" width="12" height="12" fill="#60a5fa"/>
    <rect x="24" y="48" width="12" height="12" fill="#3b82f6"/>
    <rect x="36" y="48" width="12" height="12" fill="#60a5fa"/>
    <rect x="48" y="48" width="12" height="12" fill="#f1f5f9"/>
  </g>

  <text x="130" y="96" text-anchor="middle" font-size="8" fill="#64748b">
    5×5 píxeles — la resolución está fija
  </text>
</svg>
```

---

## namespace (xmlns)

Identificador único que distingue un vocabulario XML de otro. En SVG: `http://www.w3.org/2000/svg`.

```svg
<svg width="260" height="80"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#0f172a;display:block; font-family:monospace;">

  <text x="16" y="26" font-size="9" fill="#d19a66">xmlns</text>
  <text x="56" y="26" font-size="9" fill="#56b6c2">=</text>
  <text x="65" y="26" font-size="9" fill="#98c379">"http://www.w3.org/2000/svg"</text>

  <text x="16" y="46" font-size="8" fill="#475569">
    ↑ "Este documento usa el vocabulario SVG del W3C"
  </text>

  <text x="16" y="64" font-size="8" fill="#475569">
    Obligatorio en .svg externo — opcional en HTML inline
  </text>
</svg>
```

---

## DOM (Document Object Model)

Representación del SVG como árbol de objetos en memoria. JavaScript puede leer y modificar cualquier nodo. SVG inline comparte el DOM con HTML.

```svg
<svg width="260" height="130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:monospace;">

  <!-- Árbol del DOM -->
  <text x="130" y="20" text-anchor="middle"
        font-size="10" fill="#1e293b" font-family="sans-serif" font-weight="600">
    DOM del SVG
  </text>

  <rect x="96" y="28" width="68" height="22" rx="4" fill="#1e293b"/>
  <text x="130" y="44" text-anchor="middle" font-size="9" fill="#e06c75">&lt;svg&gt;</text>

  <line x1="110" y1="50" x2="80" y2="68"  stroke="#cbd5e1" stroke-width="1"/>
  <line x1="150" y1="50" x2="180" y2="68" stroke="#cbd5e1" stroke-width="1"/>

  <rect x="44" y="68" width="72" height="22" rx="4" fill="#1e3a8a"/>
  <text x="80" y="84" text-anchor="middle" font-size="9" fill="#93c5fd">&lt;circle&gt;</text>

  <rect x="140" y="68" width="72" height="22" rx="4" fill="#14532d"/>
  <text x="176" y="84" text-anchor="middle" font-size="9" fill="#86efac">&lt;text&gt;</text>

  <line x1="80" y1="90" x2="80" y2="104" stroke="#cbd5e1" stroke-width="1"/>

  <rect x="34" y="104" width="92" height="18" rx="3" fill="#292524"/>
  <text x="80" y="117" text-anchor="middle" font-size="8" fill="#d19a66">fill="blue" r="20"</text>
</svg>
```

---

## W3C (World Wide Web Consortium)

Organismo que define los estándares web: HTML, CSS, SVG, XML. SVG 1.1 es una recomendación del W3C desde 2003.

```svg
<svg width="260" height="70"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Logos de estándares que define el W3C -->
  <rect x="14" y="15" width="50" height="40" rx="6" fill="#1e40af"/>
  <text x="39" y="41" text-anchor="middle"
        font-size="14" fill="white" font-weight="900">HTML</text>

  <rect x="74" y="15" width="50" height="40" rx="6" fill="#9333ea"/>
  <text x="99" y="41" text-anchor="middle"
        font-size="14" fill="white" font-weight="900">CSS</text>

  <rect x="134" y="15" width="50" height="40" rx="6" fill="#0891b2"/>
  <text x="159" y="41" text-anchor="middle"
        font-size="14" fill="white" font-weight="900">SVG</text>

  <rect x="194" y="15" width="50" height="40" rx="6" fill="#dc2626"/>
  <text x="219" y="41" text-anchor="middle"
        font-size="14" fill="white" font-weight="900">XML</text>
</svg>
```
