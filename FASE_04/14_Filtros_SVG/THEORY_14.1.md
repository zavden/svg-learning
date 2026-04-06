# 14.1 El elemento `<filter>`

Un filtro es un **pipeline de procesamiento de imagen**: el elemento se rasteriza a píxeles y cada primitiva aplica una operación sobre esos píxeles. La salida de una primitiva es la entrada de la siguiente.

---

## Definición y aplicación

```svg
<svg width="300" height="150"
     viewBox="0 0 300 150"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">definir en &lt;defs&gt;, aplicar con filter=</text>

  <defs>
    <filter id="mi-blur">
      <feGaussianBlur stdDeviation="3"/>
    </filter>
  </defs>

  <!-- Sin filtro -->
  <circle cx="75" cy="85" r="35" fill="#3b82f6"/>
  <text x="75" y="135" text-anchor="middle" font-size="8" fill="#64748b">sin filtro</text>

  <!-- Con filtro aplicado -->
  <circle cx="225" cy="85" r="35" fill="#3b82f6" filter="url(#mi-blur)"/>
  <text x="225" y="135" text-anchor="middle" font-size="8" fill="#64748b">filter="url(#mi-blur)"</text>
</svg>
```

---

## El pipeline: vector → bitmap → primitivas

```svg
<svg width="300" height="200"
     viewBox="0 0 300 200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">cómo trabaja un filtro internamente</text>

  <!-- Paso 1: vector -->
  <rect x="15" y="40" width="60" height="60" fill="#ede9fe" stroke="#8b5cf6" stroke-width="1.5" rx="4"/>
  <text x="45" y="58" text-anchor="middle" font-size="8" font-weight="bold" fill="#4c1d95">1. vector</text>
  <circle cx="45" cy="80" r="12" fill="#8b5cf6"/>
  <text x="45" y="115" text-anchor="middle" font-size="6" fill="#64748b">paths y formas</text>

  <!-- Flecha -->
  <text x="87" y="80" font-size="14" fill="#64748b">→</text>

  <!-- Paso 2: bitmap -->
  <rect x="105" y="40" width="60" height="60" fill="#ede9fe" stroke="#8b5cf6" stroke-width="1.5" rx="4"/>
  <text x="135" y="58" text-anchor="middle" font-size="8" font-weight="bold" fill="#4c1d95">2. rasterizar</text>
  <rect x="123" y="68" width="24" height="24" fill="#8b5cf6"/>
  <text x="135" y="115" text-anchor="middle" font-size="6" fill="#64748b">pixels (bitmap)</text>

  <text x="177" y="80" font-size="14" fill="#64748b">→</text>

  <!-- Paso 3: primitivas -->
  <rect x="195" y="40" width="90" height="60" fill="#ede9fe" stroke="#8b5cf6" stroke-width="1.5" rx="4"/>
  <text x="240" y="58" text-anchor="middle" font-size="8" font-weight="bold" fill="#4c1d95">3. primitivas</text>
  <rect x="207" y="68" width="66" height="8" fill="#c4b5fd"/>
  <text x="240" y="75" text-anchor="middle" font-size="5" fill="#4c1d95">feGaussianBlur</text>
  <rect x="207" y="79" width="66" height="8" fill="#c4b5fd"/>
  <text x="240" y="86" text-anchor="middle" font-size="5" fill="#4c1d95">feOffset</text>
  <rect x="207" y="90" width="66" height="8" fill="#c4b5fd"/>
  <text x="240" y="97" text-anchor="middle" font-size="5" fill="#4c1d95">feMerge</text>
  <text x="240" y="115" text-anchor="middle" font-size="6" fill="#64748b">cadena de efectos</text>

  <!-- Resultado -->
  <rect x="15" y="135" width="270" height="50" fill="#f8fafc" stroke="#94a3b8" stroke-dasharray="3,2" rx="4"/>
  <text x="150" y="152" text-anchor="middle" font-size="8" fill="#1e293b">4. el resultado REEMPLAZA al elemento original</text>
  <text x="150" y="168" text-anchor="middle" font-size="7" fill="#64748b">el pipeline es lineal: salida de uno → entrada del siguiente</text>
  <text x="150" y="180" text-anchor="middle" font-size="7" fill="#ef4444">los filtros son costosos: cada píxel se procesa</text>
</svg>
```

---

## Un filtro mínimo: `<filter>` con una primitiva

```svg
<svg width="300" height="150"
     viewBox="0 0 300 150"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">la estructura mínima de un filtro</text>

  <defs>
    <filter id="gris">
      <feColorMatrix type="saturate" values="0"/>
    </filter>
  </defs>

  <!-- Color original -->
  <rect x="30" y="45" width="100" height="60" fill="#ec4899" rx="6"/>
  <text x="80" y="120" text-anchor="middle" font-size="7" fill="#64748b">color original</text>

  <!-- Filtrado -->
  <rect x="170" y="45" width="100" height="60" fill="#ec4899" rx="6" filter="url(#gris)"/>
  <text x="220" y="120" text-anchor="middle" font-size="7" fill="#64748b">saturate=0 (grises)</text>

  <text x="150" y="140" text-anchor="middle" font-size="7" fill="#94a3b8">
    &lt;filter id="gris"&gt;&lt;feColorMatrix type="saturate" values="0"/&gt;&lt;/filter&gt;
  </text>
</svg>
```

---

## Región del filtro: `x`, `y`, `width`, `height`

```svg
<svg width="300" height="180"
     viewBox="0 0 300 180"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">región del filtro: el área de procesamiento</text>

  <defs>
    <!-- Filtro con área por defecto (-10% -10% 120% 120%) -->
    <filter id="f-default">
      <feGaussianBlur stdDeviation="6"/>
    </filter>
    <!-- Filtro con área muy pequeña: el blur se recorta -->
    <filter id="f-chica" x="0" y="0" width="100%" height="100%">
      <feGaussianBlur stdDeviation="6"/>
    </filter>
    <!-- Filtro con área generosa -->
    <filter id="f-grande" x="-50%" y="-50%" width="200%" height="200%">
      <feGaussianBlur stdDeviation="6"/>
    </filter>
  </defs>

  <!-- Por defecto: 10% extra en cada lado, el blur cabe bien -->
  <rect x="15" y="50" width="70" height="70" fill="#3b82f6" filter="url(#f-default)"/>
  <text x="50" y="135" text-anchor="middle" font-size="7" fill="#64748b">default</text>
  <text x="50" y="147" text-anchor="middle" font-size="6" fill="#94a3b8">120% x 120%</text>

  <!-- Chica: la blur se corta en los bordes -->
  <rect x="115" y="50" width="70" height="70" fill="#3b82f6" filter="url(#f-chica)"/>
  <text x="150" y="135" text-anchor="middle" font-size="7" fill="#ef4444">100% x 100%</text>
  <text x="150" y="147" text-anchor="middle" font-size="6" fill="#94a3b8">blur cortado</text>

  <!-- Grande: el blur cabe con margen -->
  <rect x="215" y="50" width="70" height="70" fill="#3b82f6" filter="url(#f-grande)"/>
  <text x="250" y="135" text-anchor="middle" font-size="7" fill="#10b981">200% x 200%</text>
  <text x="250" y="147" text-anchor="middle" font-size="6" fill="#94a3b8">margen amplio</text>

  <text x="150" y="168" text-anchor="middle" font-size="7" fill="#94a3b8">
    si el efecto se sale del elemento (blur, sombra), ampliar el área
  </text>
</svg>
```

---

## `filter` como atributo vs propiedad CSS

```svg
<svg width="300" height="160"
     viewBox="0 0 300 160"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <style>
    .con-css { filter: url(#f-saturado); }
  </style>

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">dos formas de aplicar un filtro</text>

  <defs>
    <filter id="f-saturado">
      <feColorMatrix type="saturate" values="2.5"/>
    </filter>
  </defs>

  <!-- Original -->
  <circle cx="60" cy="80" r="28" fill="#f59e0b"/>
  <text x="60" y="125" text-anchor="middle" font-size="7" fill="#64748b">original</text>

  <!-- Como atributo SVG -->
  <circle cx="150" cy="80" r="28" fill="#f59e0b" filter="url(#f-saturado)"/>
  <text x="150" y="125" text-anchor="middle" font-size="7" fill="#64748b">filter="url(#..)"</text>
  <text x="150" y="137" text-anchor="middle" font-size="6" fill="#94a3b8">atributo SVG</text>

  <!-- Con CSS -->
  <circle cx="240" cy="80" r="28" fill="#f59e0b" class="con-css"/>
  <text x="240" y="125" text-anchor="middle" font-size="7" fill="#64748b">filter: url(#..)</text>
  <text x="240" y="137" text-anchor="middle" font-size="6" fill="#94a3b8">propiedad CSS</text>

  <text x="150" y="155" text-anchor="middle" font-size="7" fill="#94a3b8">
    CSS además acepta blur(), brightness(), contrast(), hue-rotate()…
  </text>
</svg>
```

---

## Filtro aplicado a un grupo entero

```svg
<svg width="300" height="180"
     viewBox="0 0 300 180"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">un filter en &lt;g&gt; afecta a todo el grupo</text>

  <defs>
    <filter id="g-shadow" x="-20%" y="-20%" width="140%" height="140%">
      <feDropShadow dx="2" dy="3" stdDeviation="2" flood-color="#000" flood-opacity="0.3"/>
    </filter>
  </defs>

  <!-- Grupo sin filtro -->
  <g>
    <circle cx="55" cy="85" r="22" fill="#3b82f6"/>
    <rect x="85" y="63" width="44" height="44" rx="4" fill="#ec4899"/>
  </g>
  <text x="85" y="135" text-anchor="middle" font-size="7" fill="#64748b">grupo sin filtro</text>

  <!-- Grupo con filtro: la sombra aplica a todo el grupo como una unidad -->
  <g filter="url(#g-shadow)">
    <circle cx="195" cy="85" r="22" fill="#3b82f6"/>
    <rect x="225" y="63" width="44" height="44" rx="4" fill="#ec4899"/>
  </g>
  <text x="225" y="135" text-anchor="middle" font-size="7" fill="#64748b">grupo con drop-shadow</text>

  <text x="150" y="160" text-anchor="middle" font-size="7" fill="#94a3b8">
    el filtro se calcula sobre el grupo rasterizado como una sola imagen
  </text>
</svg>
```
