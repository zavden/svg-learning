# Referencia rápida — Imágenes y elementos embebidos

---

## Anatomía de `<image>`

```svg
<svg width="300" height="170"
     viewBox="0 0 300 170"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">atributos de &lt;image&gt;</text>

  <rect x="20" y="35" width="260" height="125" fill="white" stroke="#cbd5e1" rx="4"/>

  <text x="30" y="55" font-size="9" font-family="monospace" fill="#1e293b">&lt;image</text>
  <text x="40" y="68" font-size="8" font-family="monospace" fill="#3b82f6">href="logo.png"</text>
  <text x="130" y="68" font-size="7" fill="#64748b">← ruta o data URI</text>
  <text x="40" y="80" font-size="8" font-family="monospace" fill="#10b981">x="20" y="20"</text>
  <text x="130" y="80" font-size="7" fill="#64748b">← posición en coordenadas</text>
  <text x="40" y="92" font-size="8" font-family="monospace" fill="#ef4444">width="100" height="80"</text>
  <text x="130" y="92" font-size="7" fill="#64748b">← obligatorio</text>
  <text x="40" y="104" font-size="8" font-family="monospace" fill="#8b5cf6">preserveAspectRatio="xMidYMid meet"</text>
  <text x="40" y="116" font-size="7" fill="#64748b">   ↳ meet / slice / none</text>
  <text x="30" y="128" font-size="9" font-family="monospace" fill="#1e293b">/&gt;</text>

  <text x="30" y="150" font-size="7" fill="#475569">formatos: PNG, JPEG, GIF, WebP, SVG</text>
</svg>
```

---

## Tabla: `<image>` vs `<foreignObject>` vs SVG anidado

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">elegir la herramienta correcta</text>

  <!-- Header -->
  <rect x="15" y="30" width="270" height="18" fill="#e2e8f0"/>
  <text x="22" y="42" font-size="7" font-weight="bold" fill="#1e293b">Necesidad</text>
  <text x="170" y="42" font-size="7" font-weight="bold" fill="#1e293b">Solución</text>

  <!-- Rows -->
  <rect x="15" y="48" width="270" height="20" fill="white" stroke="#f1f5f9"/>
  <text x="22" y="61" font-size="7" fill="#475569">Foto o bitmap</text>
  <text x="170" y="61" font-size="7" font-family="monospace" fill="#3b82f6">&lt;image&gt;</text>

  <rect x="15" y="68" width="270" height="20" fill="#f8fafc" stroke="#f1f5f9"/>
  <text x="22" y="81" font-size="7" fill="#475569">Ícono SVG externo</text>
  <text x="170" y="81" font-size="7" font-family="monospace" fill="#3b82f6">&lt;image&gt;</text>

  <rect x="15" y="88" width="270" height="20" fill="white" stroke="#f1f5f9"/>
  <text x="22" y="101" font-size="7" fill="#475569">Texto multilínea</text>
  <text x="170" y="101" font-size="7" font-family="monospace" fill="#10b981">&lt;foreignObject&gt;</text>

  <rect x="15" y="108" width="270" height="20" fill="#f8fafc" stroke="#f1f5f9"/>
  <text x="22" y="121" font-size="7" fill="#475569">Formulario HTML</text>
  <text x="170" y="121" font-size="7" font-family="monospace" fill="#10b981">&lt;foreignObject&gt;</text>

  <rect x="15" y="128" width="270" height="20" fill="white" stroke="#f1f5f9"/>
  <text x="22" y="141" font-size="7" fill="#475569">Sub-sistema de coordenadas</text>
  <text x="170" y="141" font-size="7" font-family="monospace" fill="#8b5cf6">&lt;svg&gt; anidado</text>

  <rect x="15" y="148" width="270" height="20" fill="#f8fafc" stroke="#f1f5f9"/>
  <text x="22" y="161" font-size="7" fill="#475569">Agrupar sin nuevo viewport</text>
  <text x="170" y="161" font-size="7" font-family="monospace" fill="#8b5cf6">&lt;g&gt;</text>

  <rect x="15" y="168" width="270" height="20" fill="white" stroke="#f1f5f9"/>
  <text x="22" y="181" font-size="7" fill="#475569">SVG autosuficiente</text>
  <text x="170" y="181" font-size="7" font-family="monospace" fill="#f59e0b">data URI base64</text>

  <rect x="15" y="188" width="270" height="20" fill="#f8fafc" stroke="#f1f5f9"/>
  <text x="22" y="201" font-size="7" fill="#475569">Recortar zona rectangular</text>
  <text x="170" y="201" font-size="7" font-family="monospace" fill="#8b5cf6">&lt;svg&gt; anidado</text>

  <rect x="15" y="208" width="270" height="20" fill="white" stroke="#f1f5f9"/>
  <text x="22" y="221" font-size="7" fill="#475569">Tabla de datos en diagrama</text>
  <text x="170" y="221" font-size="7" font-family="monospace" fill="#10b981">&lt;foreignObject&gt;</text>

  <text x="150" y="248" text-anchor="middle" font-size="6" fill="#94a3b8">
    ¿el SVG viajará como archivo? → evita foreignObject
  </text>
</svg>
```

---

## Valores clave de `preserveAspectRatio`

```svg
<svg width="300" height="210"
     viewBox="0 0 300 210"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">preserveAspectRatio de un vistazo</text>

  <!-- Column headers -->
  <text x="55" y="42" text-anchor="middle" font-size="7" font-weight="bold" fill="#1e293b">valor</text>
  <text x="155" y="42" text-anchor="middle" font-size="7" font-weight="bold" fill="#1e293b">comportamiento</text>
  <text x="255" y="42" text-anchor="middle" font-size="7" font-weight="bold" fill="#1e293b">uso típico</text>

  <line x1="15" y1="48" x2="285" y2="48" stroke="#cbd5e1"/>

  <!-- xMidYMid meet (default) -->
  <text x="55" y="65" text-anchor="middle" font-size="7" font-family="monospace" fill="#3b82f6">xMidYMid meet</text>
  <text x="155" y="65" text-anchor="middle" font-size="6" fill="#475569">cabe entero, deja bordes</text>
  <text x="255" y="65" text-anchor="middle" font-size="6" fill="#475569">logos, íconos</text>
  <text x="55" y="74" text-anchor="middle" font-size="6" fill="#94a3b8">(por defecto)</text>

  <line x1="15" y1="85" x2="285" y2="85" stroke="#f1f5f9"/>

  <!-- xMidYMid slice -->
  <text x="55" y="102" text-anchor="middle" font-size="7" font-family="monospace" fill="#3b82f6">xMidYMid slice</text>
  <text x="155" y="102" text-anchor="middle" font-size="6" fill="#475569">llena todo, recorta sobrantes</text>
  <text x="255" y="102" text-anchor="middle" font-size="6" fill="#475569">hero, portadas</text>

  <line x1="15" y1="115" x2="285" y2="115" stroke="#f1f5f9"/>

  <!-- none -->
  <text x="55" y="132" text-anchor="middle" font-size="7" font-family="monospace" fill="#3b82f6">none</text>
  <text x="155" y="132" text-anchor="middle" font-size="6" fill="#475569">estira sin conservar ratio</text>
  <text x="255" y="132" text-anchor="middle" font-size="6" fill="#475569">fondos, texturas</text>

  <line x1="15" y1="145" x2="285" y2="145" stroke="#f1f5f9"/>

  <!-- xMinYMin meet -->
  <text x="55" y="162" text-anchor="middle" font-size="7" font-family="monospace" fill="#3b82f6">xMinYMin meet</text>
  <text x="155" y="162" text-anchor="middle" font-size="6" fill="#475569">alinea arriba-izquierda</text>
  <text x="255" y="162" text-anchor="middle" font-size="6" fill="#475569">composiciones</text>

  <line x1="15" y1="175" x2="285" y2="175" stroke="#f1f5f9"/>

  <!-- xMaxYMax slice -->
  <text x="55" y="192" text-anchor="middle" font-size="7" font-family="monospace" fill="#3b82f6">xMaxYMax slice</text>
  <text x="155" y="192" text-anchor="middle" font-size="6" fill="#475569">alinea abajo-derecha</text>
  <text x="255" y="192" text-anchor="middle" font-size="6" fill="#475569">avatares recortados</text>
</svg>
```

---

## Snippets copy-paste

### `<image>` básico con dimensiones

```svg
<image href="ruta/imagen.png" x="20" y="20" width="100" height="100"/>
```

### `<image>` con recorte circular

```svg
<clipPath id="circle-clip">
  <circle cx="50" cy="50" r="50"/>
</clipPath>
<image href="avatar.jpg" x="0" y="0" width="100" height="100"
       clip-path="url(#circle-clip)"/>
```

### `<foreignObject>` con párrafo HTML

```svg
<foreignObject x="10" y="10" width="280" height="100">
  <div xmlns="http://www.w3.org/1999/xhtml"
       style="font-family:sans-serif; font-size:12px; color:#1e293b;">
    <p>Texto con <strong>word-wrap</strong> automático y formato CSS completo.</p>
  </div>
</foreignObject>
```

### SVG anidado con viewBox propio

```svg
<svg x="50" y="50" width="100" height="100" viewBox="0 0 10 10">
  <!-- coordenadas 0–10 dentro de este SVG -->
  <circle cx="5" cy="5" r="4" fill="blue"/>
</svg>
```

### Data URI SVG inline (URL-encoded)

```svg
<image x="0" y="0" width="50" height="50"
       href="data:image/svg+xml;utf8,%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20viewBox%3D%220%200%2010%2010%22%3E%3Ccircle%20cx%3D%225%22%20cy%3D%225%22%20r%3D%224%22%20fill%3D%22red%22%2F%3E%3C%2Fsvg%3E"/>
```
