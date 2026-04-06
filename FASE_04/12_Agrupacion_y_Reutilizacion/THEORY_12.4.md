# 12.4 El elemento `<defs>`

`<defs>` es la "biblioteca" del SVG: un contenedor para cualquier cosa que se define pero no se dibuja directamente. Todo lo que vive dentro solo aparece cuando algo lo referencia.

---

## `<defs>` como biblioteca invisible

```svg
<svg width="300" height="150"
     viewBox="0 0 300 150"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">&lt;defs&gt; define, no dibuja</text>

  <defs>
    <!-- Un gradiente que se usará luego -->
    <linearGradient id="cielo" x1="0" y1="0" x2="0" y2="1">
      <stop offset="0" stop-color="#3b82f6"/>
      <stop offset="1" stop-color="#dbeafe"/>
    </linearGradient>

    <!-- Un símbolo estrella -->
    <symbol id="nube" viewBox="0 0 40 20">
      <ellipse cx="12" cy="14" rx="10" ry="6" fill="white"/>
      <ellipse cx="22" cy="10" rx="12" ry="8" fill="white"/>
      <ellipse cx="30" cy="14" rx="8" ry="5" fill="white"/>
    </symbol>
  </defs>

  <!-- Aquí empieza lo visible: el gradiente y las nubes usados -->
  <rect x="20" y="40" width="260" height="90" fill="url(#cielo)" stroke="#64748b" stroke-width="1"/>
  <use href="#nube" x="40"  y="50" width="60" height="30"/>
  <use href="#nube" x="130" y="60" width="50" height="25"/>
  <use href="#nube" x="210" y="48" width="55" height="28"/>

  <text x="150" y="145" text-anchor="middle" font-size="8" fill="#94a3b8">
    el gradiente y las nubes están en &lt;defs&gt; — aquí solo hay referencias
  </text>
</svg>
```

---

## Qué elementos suelen vivir en `<defs>`

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">el catálogo de &lt;defs&gt;</text>

  <defs>
    <linearGradient id="ej-lg" x1="0" y1="0" x2="1" y2="0">
      <stop offset="0" stop-color="#3b82f6"/>
      <stop offset="1" stop-color="#ec4899"/>
    </linearGradient>
    <radialGradient id="ej-rg">
      <stop offset="0" stop-color="#fef9c3"/>
      <stop offset="1" stop-color="#f59e0b"/>
    </radialGradient>
    <pattern id="ej-pat" width="8" height="8" patternUnits="userSpaceOnUse">
      <circle cx="4" cy="4" r="2" fill="#10b981"/>
    </pattern>
    <filter id="ej-fil">
      <feGaussianBlur stdDeviation="1.5"/>
    </filter>
    <clipPath id="ej-clip">
      <circle cx="20" cy="20" r="18"/>
    </clipPath>
    <symbol id="ej-sym" viewBox="0 0 24 24">
      <polygon points="12,3 15,10 22,10 16,15 18,22 12,18 6,22 8,15 2,10 9,10" fill="#ec4899"/>
    </symbol>
  </defs>

  <!-- linearGradient -->
  <rect x="30" y="40" width="40" height="40" fill="url(#ej-lg)"/>
  <text x="50" y="93" text-anchor="middle" font-size="7" fill="#64748b">linearGradient</text>

  <!-- radialGradient -->
  <rect x="115" y="40" width="40" height="40" fill="url(#ej-rg)"/>
  <text x="135" y="93" text-anchor="middle" font-size="7" fill="#64748b">radialGradient</text>

  <!-- pattern -->
  <rect x="200" y="40" width="40" height="40" fill="url(#ej-pat)" stroke="#64748b" stroke-width="0.5"/>
  <text x="220" y="93" text-anchor="middle" font-size="7" fill="#64748b">pattern</text>

  <!-- filter -->
  <rect x="30" y="115" width="40" height="40" fill="#3b82f6" filter="url(#ej-fil)"/>
  <text x="50" y="168" text-anchor="middle" font-size="7" fill="#64748b">filter</text>

  <!-- clipPath -->
  <rect x="115" y="115" width="40" height="40" fill="#f59e0b" clip-path="url(#ej-clip)"/>
  <text x="135" y="168" text-anchor="middle" font-size="7" fill="#64748b">clipPath</text>

  <!-- symbol + use -->
  <use href="#ej-sym" x="200" y="115" width="40" height="40"/>
  <text x="220" y="168" text-anchor="middle" font-size="7" fill="#64748b">symbol</text>

  <text x="150" y="200" text-anchor="middle" font-size="8" fill="#94a3b8">
    gradientes, patrones, filtros, clipPath, mask, símbolos, markers…
  </text>
</svg>
```

---

## `<defs>` al inicio vs más adelante: el orden no importa

```svg
<svg width="300" height="150"
     viewBox="0 0 300 150"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">&lt;defs&gt; al inicio por convención</text>

  <!-- Se puede referenciar un gradiente antes de definirlo: es válido -->
  <rect x="20" y="40" width="120" height="80" fill="url(#retro-gradient)" stroke="#64748b"/>
  <text x="80" y="135" text-anchor="middle" font-size="8" fill="#64748b">usa el gradiente…</text>

  <!-- La definición aparece después — el navegador sí lo resuelve -->
  <defs>
    <linearGradient id="retro-gradient" x1="0" y1="0" x2="1" y2="1">
      <stop offset="0" stop-color="#f59e0b"/>
      <stop offset="0.5" stop-color="#ec4899"/>
      <stop offset="1" stop-color="#8b5cf6"/>
    </linearGradient>
  </defs>

  <text x="220" y="70" font-size="9" fill="#1e293b">el gradiente está</text>
  <text x="220" y="84" font-size="9" fill="#1e293b">definido DESPUÉS</text>
  <text x="220" y="105" font-size="8" fill="#10b981">✓ sigue funcionando</text>
</svg>
```

---

## Un `<defs>` compartido entre varios elementos

```svg
<svg width="300" height="190"
     viewBox="0 0 300 190"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">una definición, muchos usos</text>

  <!-- Un gradiente, un filtro y un símbolo se usan en varios lugares -->
  <defs>
    <linearGradient id="btn-fill" x1="0" y1="0" x2="0" y2="1">
      <stop offset="0" stop-color="#8b5cf6"/>
      <stop offset="1" stop-color="#4c1d95"/>
    </linearGradient>

    <filter id="btn-shadow" x="-20%" y="-20%" width="140%" height="160%">
      <feGaussianBlur stdDeviation="2"/>
      <feOffset dy="2"/>
      <feComponentTransfer><feFuncA type="linear" slope="0.4"/></feComponentTransfer>
      <feComposite in2="SourceGraphic" operator="out"/>
    </filter>

    <symbol id="play" viewBox="0 0 24 24">
      <polygon points="7,4 20,12 7,20" fill="white"/>
    </symbol>
  </defs>

  <!-- Tres botones que comparten el mismo gradiente, filtro y símbolo -->
  <g transform="translate(30, 50)">
    <rect x="0" y="0" width="70" height="40" rx="8" fill="url(#btn-fill)"/>
    <use href="#play" x="10" y="8" width="24" height="24"/>
    <text x="50" y="26" font-size="10" fill="white">Play</text>
  </g>

  <g transform="translate(115, 50)">
    <rect x="0" y="0" width="70" height="40" rx="8" fill="url(#btn-fill)"/>
    <use href="#play" x="10" y="8" width="24" height="24"/>
    <text x="50" y="26" font-size="10" fill="white">Play</text>
  </g>

  <g transform="translate(200, 50)">
    <rect x="0" y="0" width="70" height="40" rx="8" fill="url(#btn-fill)"/>
    <use href="#play" x="10" y="8" width="24" height="24"/>
    <text x="50" y="26" font-size="10" fill="white">Play</text>
  </g>

  <text x="150" y="120" text-anchor="middle" font-size="8" fill="#64748b">3 botones — 1 gradiente + 1 símbolo</text>

  <rect x="30" y="135" width="240" height="40" fill="#fef9c3" stroke="#f59e0b" rx="4"/>
  <text x="40" y="152" font-size="8" fill="#92400e" font-weight="bold">Ventaja:</text>
  <text x="40" y="165" font-size="8" fill="#92400e">cambiar el gradiente en &lt;defs&gt; actualiza los 3 botones</text>
</svg>
```

---

## Cuándo usar `<defs>` y cuándo no

```svg
<svg width="300" height="190"
     viewBox="0 0 300 190"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">guía rápida: &lt;defs&gt; sí o no</text>

  <!-- Dentro de defs -->
  <rect x="15" y="35" width="130" height="140" fill="#dcfce7" stroke="#10b981" stroke-width="1.5" rx="4"/>
  <text x="80" y="52" text-anchor="middle" font-size="10" font-weight="bold" fill="#166534">Dentro de &lt;defs&gt;</text>

  <text x="25" y="72"  font-size="7" fill="#1e293b">✓ gradientes, patterns</text>
  <text x="25" y="85"  font-size="7" fill="#1e293b">✓ filtros, clipPath, mask</text>
  <text x="25" y="98"  font-size="7" fill="#1e293b">✓ símbolos de icono</text>
  <text x="25" y="111" font-size="7" fill="#1e293b">✓ grupos referenciados con use</text>
  <text x="25" y="124" font-size="7" fill="#1e293b">✓ paths para textPath</text>
  <text x="25" y="137" font-size="7" fill="#1e293b">✓ marcadores (marker)</text>
  <text x="25" y="150" font-size="7" fill="#166534">→ algo que se define una</text>
  <text x="25" y="160" font-size="7" fill="#166534">   vez y se referencia</text>

  <!-- Fuera de defs -->
  <rect x="155" y="35" width="130" height="140" fill="#fef2f2" stroke="#ef4444" stroke-width="1.5" rx="4"/>
  <text x="220" y="52" text-anchor="middle" font-size="10" font-weight="bold" fill="#9d174d">Fuera de &lt;defs&gt;</text>

  <text x="165" y="72"  font-size="7" fill="#1e293b">✓ formas visibles del dibujo</text>
  <text x="165" y="85"  font-size="7" fill="#1e293b">✓ grupos que se renderizan</text>
  <text x="165" y="98"  font-size="7" fill="#1e293b">✓ textos y títulos</text>
  <text x="165" y="111" font-size="7" fill="#1e293b">✓ elementos &lt;use&gt;</text>
  <text x="165" y="124" font-size="7" fill="#1e293b">✓ lo que pinta el canvas</text>
  <text x="165" y="150" font-size="7" fill="#9d174d">→ todo lo que el usuario</text>
  <text x="165" y="160" font-size="7" fill="#9d174d">   debe ver directamente</text>
</svg>
```
