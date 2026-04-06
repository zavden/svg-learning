# 14.3 Entradas de filtro predefinidas

El atributo `in` de cada primitiva indica qué imagen usar como entrada. SVG ofrece varios **valores predefinidos** que no requieren definición previa: `SourceGraphic`, `SourceAlpha`, `FillPaint`, `StrokePaint`, etc.

---

## `SourceGraphic` — el elemento completo

```svg
<svg width="300" height="160"
     viewBox="0 0 300 160"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">SourceGraphic: el elemento tal cual</text>

  <defs>
    <filter id="f-blur-graphic">
      <!-- in="SourceGraphic" es el valor por defecto cuando no se indica nada -->
      <feGaussianBlur in="SourceGraphic" stdDeviation="3"/>
    </filter>
  </defs>

  <!-- Grupo colorido -->
  <g>
    <circle cx="70" cy="80" r="22" fill="#3b82f6"/>
    <rect x="55" y="65" width="30" height="30" fill="#ec4899" opacity="0.7"/>
    <polygon points="80,55 95,85 60,85" fill="#f59e0b"/>
  </g>
  <text x="75" y="125" text-anchor="middle" font-size="7" fill="#64748b">original</text>

  <!-- Blur usando SourceGraphic: los colores se preservan -->
  <g filter="url(#f-blur-graphic)">
    <circle cx="215" cy="80" r="22" fill="#3b82f6"/>
    <rect x="200" y="65" width="30" height="30" fill="#ec4899" opacity="0.7"/>
    <polygon points="225,55 240,85 205,85" fill="#f59e0b"/>
  </g>
  <text x="220" y="125" text-anchor="middle" font-size="7" fill="#64748b">blur sobre SourceGraphic</text>

  <text x="150" y="148" text-anchor="middle" font-size="7" fill="#94a3b8">
    los colores originales están presentes en el blur
  </text>
</svg>
```

---

## `SourceAlpha` — solo la silueta, sin color

```svg
<svg width="300" height="170"
     viewBox="0 0 300 170"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">SourceAlpha: la silueta en negro</text>

  <defs>
    <filter id="f-silueta">
      <!-- SourceAlpha: solo el canal alfa, sin colores -->
      <feGaussianBlur in="SourceAlpha" stdDeviation="2"/>
    </filter>
  </defs>

  <g>
    <circle cx="70" cy="80" r="22" fill="#3b82f6"/>
    <rect x="55" y="65" width="30" height="30" fill="#ec4899" opacity="0.7"/>
    <polygon points="80,55 95,85 60,85" fill="#f59e0b"/>
  </g>
  <text x="75" y="125" text-anchor="middle" font-size="7" fill="#64748b">SourceGraphic</text>
  <text x="75" y="137" text-anchor="middle" font-size="6" fill="#94a3b8">con colores</text>

  <g filter="url(#f-silueta)">
    <circle cx="215" cy="80" r="22" fill="#3b82f6"/>
    <rect x="200" y="65" width="30" height="30" fill="#ec4899" opacity="0.7"/>
    <polygon points="225,55 240,85 205,85" fill="#f59e0b"/>
  </g>
  <text x="220" y="125" text-anchor="middle" font-size="7" fill="#64748b">SourceAlpha</text>
  <text x="220" y="137" text-anchor="middle" font-size="6" fill="#94a3b8">solo la silueta negra</text>

  <text x="150" y="158" text-anchor="middle" font-size="7" fill="#94a3b8">
    ideal para sombras: no revela los colores internos
  </text>
</svg>
```

---

## Por qué `SourceAlpha` es clave en sombras

```svg
<svg width="300" height="190"
     viewBox="0 0 300 190"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">sombra con SourceGraphic vs SourceAlpha</text>

  <defs>
    <!-- MAL: usar SourceGraphic → la sombra conserva los colores del elemento -->
    <filter id="sombra-graphic" x="-50%" y="-50%" width="200%" height="200%">
      <feGaussianBlur in="SourceGraphic" stdDeviation="4" result="blur"/>
      <feOffset in="blur" dx="5" dy="6" result="sombra"/>
      <feMerge>
        <feMergeNode in="sombra"/>
        <feMergeNode in="SourceGraphic"/>
      </feMerge>
    </filter>

    <!-- BIEN: usar SourceAlpha → la sombra es solo una silueta oscura -->
    <filter id="sombra-alpha" x="-50%" y="-50%" width="200%" height="200%">
      <feGaussianBlur in="SourceAlpha" stdDeviation="4" result="blur"/>
      <feOffset in="blur" dx="5" dy="6" result="sombra"/>
      <feMerge>
        <feMergeNode in="sombra"/>
        <feMergeNode in="SourceGraphic"/>
      </feMerge>
    </filter>
  </defs>

  <rect x="15" y="35" width="130" height="140" fill="#fef2f2" stroke="#ef4444" stroke-width="1.5" rx="4"/>
  <text x="80" y="52" text-anchor="middle" font-size="9" fill="#9d174d" font-weight="bold">SourceGraphic</text>
  <g filter="url(#sombra-graphic)" transform="translate(25, 60)">
    <circle cx="35" cy="35" r="20" fill="#3b82f6"/>
    <rect x="15" y="15" width="25" height="25" fill="#ec4899" opacity="0.7"/>
  </g>
  <text x="80" y="162" text-anchor="middle" font-size="7" fill="#ef4444">sombra coloreada (raro)</text>

  <rect x="155" y="35" width="130" height="140" fill="#dcfce7" stroke="#10b981" stroke-width="1.5" rx="4"/>
  <text x="220" y="52" text-anchor="middle" font-size="9" fill="#166534" font-weight="bold">SourceAlpha</text>
  <g filter="url(#sombra-alpha)" transform="translate(165, 60)">
    <circle cx="35" cy="35" r="20" fill="#3b82f6"/>
    <rect x="15" y="15" width="25" height="25" fill="#ec4899" opacity="0.7"/>
  </g>
  <text x="220" y="162" text-anchor="middle" font-size="7" fill="#166534">sombra neutra (correcto)</text>
</svg>
```

---

## `FillPaint` y `StrokePaint`

```svg
<svg width="300" height="170"
     viewBox="0 0 300 170"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">FillPaint y StrokePaint</text>

  <defs>
    <!-- Un filtro que muestra solo el "color de relleno" como imagen plena -->
    <filter id="solo-fill">
      <feFlood flood-color="#ec4899"/>
      <feComposite in2="SourceAlpha" operator="in"/>
    </filter>
  </defs>

  <!-- Elemento con fill y stroke -->
  <circle cx="75" cy="80" r="28" fill="#3b82f6" stroke="#1e40af" stroke-width="4"/>
  <text x="75" y="125" text-anchor="middle" font-size="7" fill="#64748b">original</text>
  <text x="75" y="137" text-anchor="middle" font-size="6" fill="#94a3b8">fill + stroke</text>

  <!-- Tinte rosa forzado -->
  <circle cx="220" cy="80" r="28" fill="#3b82f6" stroke="#1e40af" stroke-width="4"
          filter="url(#solo-fill)"/>
  <text x="220" y="125" text-anchor="middle" font-size="7" fill="#64748b">flood + SourceAlpha</text>
  <text x="220" y="137" text-anchor="middle" font-size="6" fill="#94a3b8">todo el elemento en rosa</text>

  <text x="150" y="160" text-anchor="middle" font-size="7" fill="#94a3b8">
    FillPaint y StrokePaint son útiles cuando el fill es un gradiente/pattern
  </text>
</svg>
```

---

## Tabla resumen de entradas predefinidas

```svg
<svg width="300" height="230"
     viewBox="0 0 300 230"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" fill="#f8fafc" font-weight="bold">
    Entradas predefinidas de filter
  </text>

  <!-- SourceGraphic -->
  <rect x="10" y="34" width="280" height="32" fill="#dbeafe" stroke="#3b82f6" stroke-width="1" rx="3"/>
  <text x="20" y="48" font-size="8" font-weight="bold" fill="#1e40af">SourceGraphic</text>
  <text x="20" y="60" font-size="7" fill="#64748b">el elemento original completo (fill, stroke, colores)</text>

  <!-- SourceAlpha -->
  <rect x="10" y="72" width="280" height="32" fill="#dcfce7" stroke="#10b981" stroke-width="1" rx="3"/>
  <text x="20" y="86" font-size="8" font-weight="bold" fill="#166534">SourceAlpha</text>
  <text x="20" y="98" font-size="7" fill="#64748b">solo el canal alfa — silueta negra — ideal para sombras</text>

  <!-- BackgroundImage -->
  <rect x="10" y="110" width="280" height="32" fill="#fef9c3" stroke="#f59e0b" stroke-width="1" rx="3"/>
  <text x="20" y="124" font-size="8" font-weight="bold" fill="#92400e">BackgroundImage</text>
  <text x="20" y="136" font-size="7" fill="#64748b">la imagen del fondo (soporte limitado, requiere enable-background)</text>

  <!-- BackgroundAlpha -->
  <rect x="10" y="148" width="280" height="32" fill="#fef9c3" stroke="#f59e0b" stroke-width="1" rx="3"/>
  <text x="20" y="162" font-size="8" font-weight="bold" fill="#92400e">BackgroundAlpha</text>
  <text x="20" y="174" font-size="7" fill="#64748b">el canal alfa del fondo (soporte limitado)</text>

  <!-- FillPaint / StrokePaint -->
  <rect x="10" y="186" width="280" height="36" fill="#fce7f3" stroke="#ec4899" stroke-width="1" rx="3"/>
  <text x="20" y="200" font-size="8" font-weight="bold" fill="#9d174d">FillPaint · StrokePaint</text>
  <text x="20" y="212" font-size="7" fill="#64748b">el color/gradiente/pattern de fill o stroke como imagen plena</text>
</svg>
```

---

## `in` por defecto: la primitiva anterior

```svg
<svg width="300" height="190"
     viewBox="0 0 300 190"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">si omites "in", se toma la primitiva anterior</text>

  <defs>
    <filter id="encadenado" x="-30%" y="-30%" width="160%" height="160%">
      <!-- Sin especificar "in": blur recibe SourceGraphic por defecto -->
      <feGaussianBlur stdDeviation="3"/>
      <!-- Sin "in": toma la salida del blur -->
      <feOffset dx="4" dy="4"/>
    </filter>
  </defs>

  <circle cx="80" cy="85" r="28" fill="#94a3b8" opacity="0.4"/>
  <circle cx="80" cy="85" r="28" fill="#8b5cf6" filter="url(#encadenado)"/>
  <text x="80" y="140" text-anchor="middle" font-size="7" fill="#64748b">cadena implícita</text>

  <!-- Código equivalente -->
  <g transform="translate(150, 40)">
    <rect x="0" y="0" width="140" height="100" fill="#f1f5f9" stroke="#94a3b8" rx="4"/>
    <text x="10" y="18" font-size="7" font-family="monospace" fill="#1e293b">&lt;filter id="..."&gt;</text>
    <text x="10" y="32" font-size="7" font-family="monospace" fill="#64748b">  &lt;feGaussianBlur</text>
    <text x="10" y="42" font-size="7" font-family="monospace" fill="#64748b">      stdDeviation="3"/&gt;</text>
    <text x="10" y="56" font-size="7" font-family="monospace" fill="#64748b">  &lt;feOffset</text>
    <text x="10" y="66" font-size="7" font-family="monospace" fill="#64748b">      dx="4" dy="4"/&gt;</text>
    <text x="10" y="80" font-size="7" font-family="monospace" fill="#1e293b">&lt;/filter&gt;</text>
    <text x="10" y="94" font-size="6" fill="#10b981">✓ cada paso hereda del anterior</text>
  </g>

  <text x="150" y="175" text-anchor="middle" font-size="7" fill="#94a3b8">
    para ramas o referencias explícitas, sí hay que usar in="result-name"
  </text>
</svg>
```
