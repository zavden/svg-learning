# 14.4 Encadenamiento de filtros

Las primitivas se conectan como un grafo: cada una produce una salida nombrada con `result` y otra primitiva la consume con `in`. Si se omite `in`, se usa la salida de la primitiva anterior en el código fuente.

---

## Cadena lineal simple

```svg
<svg width="300" height="180"
     viewBox="0 0 300 180"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">pipeline lineal: paso a paso</text>

  <defs>
    <filter id="paso-1">
      <feGaussianBlur in="SourceAlpha" stdDeviation="3"/>
    </filter>
    <filter id="paso-2" x="-50%" y="-50%" width="200%" height="200%">
      <feGaussianBlur in="SourceAlpha" stdDeviation="3" result="blur"/>
      <feOffset in="blur" dx="6" dy="8"/>
    </filter>
    <filter id="paso-3" x="-50%" y="-50%" width="200%" height="200%">
      <feGaussianBlur in="SourceAlpha" stdDeviation="3" result="blur"/>
      <feOffset in="blur" dx="6" dy="8" result="offblur"/>
      <feMerge>
        <feMergeNode in="offblur"/>
        <feMergeNode in="SourceGraphic"/>
      </feMerge>
    </filter>
  </defs>

  <!-- Paso 1: solo blur -->
  <g transform="translate(25, 45)">
    <circle cx="30" cy="30" r="22" fill="#3b82f6" filter="url(#paso-1)"/>
    <text x="30" y="80" text-anchor="middle" font-size="7" fill="#1e293b" font-weight="bold">paso 1</text>
    <text x="30" y="90" text-anchor="middle" font-size="6" fill="#64748b">blur silueta</text>
  </g>

  <text x="105" y="80" font-size="14" fill="#64748b">→</text>

  <!-- Paso 2: blur + offset -->
  <g transform="translate(120, 45)">
    <circle cx="30" cy="30" r="22" fill="#3b82f6" filter="url(#paso-2)"/>
    <text x="30" y="80" text-anchor="middle" font-size="7" fill="#1e293b" font-weight="bold">paso 2</text>
    <text x="30" y="90" text-anchor="middle" font-size="6" fill="#64748b">+ offset</text>
  </g>

  <text x="200" y="80" font-size="14" fill="#64748b">→</text>

  <!-- Paso 3: blur + offset + merge -->
  <g transform="translate(215, 45)">
    <circle cx="30" cy="30" r="22" fill="#3b82f6" filter="url(#paso-3)"/>
    <text x="30" y="80" text-anchor="middle" font-size="7" fill="#1e293b" font-weight="bold">paso 3</text>
    <text x="30" y="90" text-anchor="middle" font-size="6" fill="#64748b">+ merge</text>
  </g>

  <text x="150" y="130" text-anchor="middle" font-size="7" fill="#94a3b8">
    cada paso añade una primitiva al pipeline anterior
  </text>
  <text x="150" y="150" text-anchor="middle" font-size="7" fill="#1e293b" font-weight="bold">
    resultado final: una sombra proyectada estándar
  </text>
</svg>
```

---

## `result` e `in`: nombrando salidas

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" fill="#f8fafc" font-weight="bold">
    result / in: el cableado
  </text>

  <!-- Visualización del grafo -->
  <defs>
    <marker id="arrow-dag" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="6" markerHeight="6" orient="auto">
      <path d="M 0 0 L 10 5 L 0 10 Z" fill="#64748b"/>
    </marker>
  </defs>

  <!-- Source -->
  <rect x="15" y="50" width="80" height="26" rx="4" fill="#fef9c3" stroke="#f59e0b" stroke-width="1.5"/>
  <text x="55" y="67" text-anchor="middle" font-size="8" font-weight="bold" fill="#92400e">SourceAlpha</text>

  <!-- Blur -->
  <rect x="115" y="50" width="90" height="26" rx="4" fill="#dbeafe" stroke="#3b82f6" stroke-width="1.5"/>
  <text x="160" y="65" text-anchor="middle" font-size="7" font-weight="bold" fill="#1e40af">feGaussianBlur</text>
  <text x="160" y="73" text-anchor="middle" font-size="6" fill="#64748b">result="blur"</text>

  <line x1="95" y1="63" x2="115" y2="63" stroke="#64748b" stroke-width="1.5" marker-end="url(#arrow-dag)"/>

  <!-- Offset -->
  <rect x="225" y="50" width="65" height="26" rx="4" fill="#dbeafe" stroke="#3b82f6" stroke-width="1.5"/>
  <text x="257" y="65" text-anchor="middle" font-size="7" font-weight="bold" fill="#1e40af">feOffset</text>
  <text x="257" y="73" text-anchor="middle" font-size="6" fill="#64748b">in="blur"</text>

  <line x1="205" y1="63" x2="225" y2="63" stroke="#64748b" stroke-width="1.5" marker-end="url(#arrow-dag)"/>

  <!-- Offset result -->
  <text x="257" y="92" text-anchor="middle" font-size="6" fill="#64748b">result="offblur"</text>

  <!-- Flood -->
  <rect x="15" y="110" width="80" height="26" rx="4" fill="#fce7f3" stroke="#ec4899" stroke-width="1.5"/>
  <text x="55" y="125" text-anchor="middle" font-size="7" font-weight="bold" fill="#9d174d">feFlood</text>
  <text x="55" y="133" text-anchor="middle" font-size="6" fill="#64748b">result="color"</text>

  <!-- Composite -->
  <rect x="115" y="110" width="90" height="26" rx="4" fill="#dbeafe" stroke="#3b82f6" stroke-width="1.5"/>
  <text x="160" y="122" text-anchor="middle" font-size="7" font-weight="bold" fill="#1e40af">feComposite</text>
  <text x="160" y="131" text-anchor="middle" font-size="6" fill="#64748b">in="color" in2="offblur"</text>

  <line x1="95"  y1="123" x2="115" y2="123" stroke="#64748b" stroke-width="1.5" marker-end="url(#arrow-dag)"/>
  <line x1="257" y1="90"  x2="257" y2="100" stroke="#64748b" stroke-width="1.5"/>
  <line x1="257" y1="100" x2="195" y2="100" stroke="#64748b" stroke-width="1.5"/>
  <line x1="195" y1="100" x2="195" y2="110" stroke="#64748b" stroke-width="1.5" marker-end="url(#arrow-dag)"/>

  <!-- Merge -->
  <rect x="80" y="160" width="140" height="40" rx="4" fill="#dcfce7" stroke="#10b981" stroke-width="1.5"/>
  <text x="150" y="174" text-anchor="middle" font-size="8" font-weight="bold" fill="#166534">feMerge</text>
  <text x="150" y="186" text-anchor="middle" font-size="6" fill="#64748b">sombra (abajo) + SourceGraphic (arriba)</text>

  <line x1="160" y1="136" x2="130" y2="160" stroke="#64748b" stroke-width="1.5" marker-end="url(#arrow-dag)"/>

  <text x="150" y="215" text-anchor="middle" font-size="7" fill="#94a3b8">
    el grafo puede bifurcar, unirse, tener nodos múltiples
  </text>
</svg>
```

---

## Bifurcación: una primitiva usada por varias

```svg
<svg width="300" height="200"
     viewBox="0 0 300 200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">una primitiva, varios consumidores</text>

  <defs>
    <!-- Doble sombra: una arriba-izquierda y otra abajo-derecha, ambas del mismo blur -->
    <filter id="doble-sombra" x="-50%" y="-50%" width="200%" height="200%">
      <!-- Blur común que se reutilizará dos veces -->
      <feGaussianBlur in="SourceAlpha" stdDeviation="3" result="blur-base"/>

      <!-- Rama 1: sombra arriba-izquierda color 1 -->
      <feOffset in="blur-base" dx="-4" dy="-4" result="s1-offset"/>
      <feFlood flood-color="#ec4899" flood-opacity="0.6" result="s1-color"/>
      <feComposite in="s1-color" in2="s1-offset" operator="in" result="s1"/>

      <!-- Rama 2: sombra abajo-derecha color 2 -->
      <feOffset in="blur-base" dx="4" dy="4" result="s2-offset"/>
      <feFlood flood-color="#3b82f6" flood-opacity="0.6" result="s2-color"/>
      <feComposite in="s2-color" in2="s2-offset" operator="in" result="s2"/>

      <!-- Unir las dos sombras + el original -->
      <feMerge>
        <feMergeNode in="s1"/>
        <feMergeNode in="s2"/>
        <feMergeNode in="SourceGraphic"/>
      </feMerge>
    </filter>
  </defs>

  <!-- Elemento con el filtro -->
  <rect x="100" y="65" width="100" height="60" rx="10" fill="#fef9c3" stroke="#92400e" stroke-width="2"
        filter="url(#doble-sombra)"/>

  <text x="150" y="160" text-anchor="middle" font-size="7" fill="#94a3b8">
    result="blur-base" se consume dos veces (dos ramas con offset distinto)
  </text>
  <text x="150" y="175" text-anchor="middle" font-size="7" fill="#94a3b8">
    así no se recalcula el blur: se comparte entre las dos sombras
  </text>
  <text x="150" y="190" text-anchor="middle" font-size="7" fill="#1e293b" font-weight="bold">
    patrón común: sombra doble de colores
  </text>
</svg>
```

---

## Orden de ejecución: nunca referenciar hacia adelante

```svg
<svg width="300" height="180"
     viewBox="0 0 300 180"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">orden importa: las primitivas se leen de arriba a abajo</text>

  <!-- MAL -->
  <rect x="15" y="30" width="130" height="135" fill="#fef2f2" stroke="#ef4444" stroke-width="1.5" rx="4"/>
  <text x="80" y="46" text-anchor="middle" font-size="9" font-weight="bold" fill="#9d174d">MAL</text>
  <text x="22" y="62" font-size="6.5" font-family="monospace" fill="#1e293b">&lt;feOffset in="blur"/&gt;</text>
  <text x="22" y="74" font-size="6.5" font-family="monospace" fill="#1e293b">&lt;feGaussianBlur</text>
  <text x="22" y="84" font-size="6.5" font-family="monospace" fill="#1e293b">  result="blur"/&gt;</text>
  <text x="22" y="102" font-size="6" fill="#ef4444">referencia hacia adelante</text>
  <text x="22" y="114" font-size="6" fill="#ef4444">comportamiento indefinido</text>
  <text x="22" y="126" font-size="6" fill="#ef4444">algunos navegadores lo</text>
  <text x="22" y="136" font-size="6" fill="#ef4444">resuelven, otros no</text>

  <!-- BIEN -->
  <rect x="155" y="30" width="130" height="135" fill="#dcfce7" stroke="#10b981" stroke-width="1.5" rx="4"/>
  <text x="220" y="46" text-anchor="middle" font-size="9" font-weight="bold" fill="#166534">BIEN</text>
  <text x="162" y="62" font-size="6.5" font-family="monospace" fill="#1e293b">&lt;feGaussianBlur</text>
  <text x="162" y="72" font-size="6.5" font-family="monospace" fill="#1e293b">  result="blur"/&gt;</text>
  <text x="162" y="86" font-size="6.5" font-family="monospace" fill="#1e293b">&lt;feOffset</text>
  <text x="162" y="96" font-size="6.5" font-family="monospace" fill="#1e293b">  in="blur"/&gt;</text>
  <text x="162" y="114" font-size="6" fill="#10b981">cada referencia apunta a</text>
  <text x="162" y="124" font-size="6" fill="#10b981">algo que ya existe</text>
  <text x="162" y="138" font-size="6" fill="#10b981">flujo claro y predecible</text>
</svg>
```

---

## Patrón de doble blur: suavizado extra

```svg
<svg width="300" height="170"
     viewBox="0 0 300 170"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">encadenar primitivas del mismo tipo</text>

  <defs>
    <filter id="blur-simple">
      <feGaussianBlur stdDeviation="3"/>
    </filter>

    <filter id="blur-doble">
      <!-- Dos blurs seguidos: efecto acumulativo -->
      <feGaussianBlur stdDeviation="2" result="b1"/>
      <feGaussianBlur in="b1" stdDeviation="2"/>
    </filter>

    <filter id="blur-triple">
      <feGaussianBlur stdDeviation="2" result="b1"/>
      <feGaussianBlur in="b1" stdDeviation="2" result="b2"/>
      <feGaussianBlur in="b2" stdDeviation="2"/>
    </filter>
  </defs>

  <circle cx="55"  cy="80" r="25" fill="#8b5cf6" filter="url(#blur-simple)"/>
  <text x="55"  y="125" text-anchor="middle" font-size="7" fill="#64748b">1 blur × 3</text>

  <circle cx="150" cy="80" r="25" fill="#8b5cf6" filter="url(#blur-doble)"/>
  <text x="150" y="125" text-anchor="middle" font-size="7" fill="#64748b">2 blurs × 2</text>

  <circle cx="245" cy="80" r="25" fill="#8b5cf6" filter="url(#blur-triple)"/>
  <text x="245" y="125" text-anchor="middle" font-size="7" fill="#64748b">3 blurs × 2</text>

  <text x="150" y="150" text-anchor="middle" font-size="7" fill="#94a3b8">
    blurs encadenados = blur más uniforme (los bordes quedan más suaves)
  </text>
</svg>
```

---

## Nombres `result` descriptivos: código legible

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" fill="#f8fafc" font-weight="bold">
    Nombres de result: legibilidad
  </text>

  <!-- MAL: nombres genéricos -->
  <rect x="10" y="34" width="135" height="175" fill="#fef2f2" stroke="#ef4444" stroke-width="1.5" rx="4"/>
  <text x="77" y="52" text-anchor="middle" font-size="9" font-weight="bold" fill="#9d174d">Nombres opacos</text>
  <text x="17" y="70" font-size="6" font-family="monospace" fill="#1e293b">result="r1"</text>
  <text x="17" y="82" font-size="6" font-family="monospace" fill="#1e293b">result="r2"</text>
  <text x="17" y="94" font-size="6" font-family="monospace" fill="#1e293b">result="r3"</text>
  <text x="17" y="106" font-size="6" font-family="monospace" fill="#1e293b">in="r2" in2="r3"</text>

  <text x="17" y="130" font-size="6" fill="#ef4444">× difícil de mantener</text>
  <text x="17" y="142" font-size="6" fill="#ef4444">× mover primitivas rompe</text>
  <text x="17" y="152" font-size="6" fill="#ef4444">  referencias</text>
  <text x="17" y="166" font-size="6" fill="#ef4444">× imposible entender sin</text>
  <text x="17" y="176" font-size="6" fill="#ef4444">  leer la cadena entera</text>

  <!-- BIEN: nombres semánticos -->
  <rect x="155" y="34" width="135" height="175" fill="#dcfce7" stroke="#10b981" stroke-width="1.5" rx="4"/>
  <text x="222" y="52" text-anchor="middle" font-size="9" font-weight="bold" fill="#166534">Nombres semánticos</text>
  <text x="162" y="70" font-size="6" font-family="monospace" fill="#1e293b">result="silueta-blur"</text>
  <text x="162" y="82" font-size="6" font-family="monospace" fill="#1e293b">result="sombra-desp"</text>
  <text x="162" y="94" font-size="6" font-family="monospace" fill="#1e293b">result="sombra-color"</text>
  <text x="162" y="106" font-size="6" font-family="monospace" fill="#1e293b">in="sombra-color"</text>

  <text x="162" y="130" font-size="6" fill="#10b981">✓ autodocumenta la cadena</text>
  <text x="162" y="142" font-size="6" fill="#10b981">✓ facil de mantener</text>
  <text x="162" y="154" font-size="6" fill="#10b981">✓ facil de depurar</text>
  <text x="162" y="166" font-size="6" fill="#10b981">✓ reorganizar no rompe</text>
  <text x="162" y="176" font-size="6" fill="#10b981">  las referencias</text>
</svg>
```
