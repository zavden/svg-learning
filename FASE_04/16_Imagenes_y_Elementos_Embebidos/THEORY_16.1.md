# 16.1 El elemento `<image>`

SVG no es sólo vectores: el elemento `<image>` incrusta imágenes rasterizadas (PNG, JPEG, GIF, WebP) u otros SVG dentro del canvas. Se comporta como un "rectángulo que contiene una imagen", con posición y tamaño propios.

---

## Sintaxis mínima

```svg
<svg width="300" height="180"
     viewBox="0 0 300 180"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">href + x + y + width + height</text>

  <!-- Imagen de prueba como data URI (para que el ejemplo sea autocontenido) -->
  <image href="data:image/svg+xml;utf8,&lt;svg xmlns='http://www.w3.org/2000/svg' width='80' height='80'&gt;&lt;rect width='80' height='80' fill='%238b5cf6'/&gt;&lt;circle cx='40' cy='40' r='28' fill='%23fef9c3'/&gt;&lt;text x='40' y='46' text-anchor='middle' font-size='22' font-family='sans-serif' fill='%238b5cf6'&gt;IMG&lt;/text&gt;&lt;/svg&gt;"
         x="40" y="50"
         width="80" height="80"/>

  <!-- Anotaciones -->
  <line x1="40" y1="50" x2="15" y2="50" stroke="#64748b" stroke-dasharray="2,2"/>
  <text x="10" y="53" font-size="6" text-anchor="end" fill="#64748b">y=50</text>

  <line x1="40" y1="130" x2="15" y2="130" stroke="#64748b" stroke-dasharray="2,2"/>
  <text x="10" y="133" font-size="6" text-anchor="end" fill="#64748b">h=80</text>

  <text x="80" y="45" text-anchor="middle" font-size="6" fill="#64748b">x=40 w=80</text>

  <!-- Código -->
  <rect x="150" y="50" width="135" height="90" fill="#f1f5f9" stroke="#cbd5e1" rx="4"/>
  <text x="160" y="68" font-size="7" font-family="monospace" fill="#1e293b">&lt;image</text>
  <text x="160" y="80" font-size="7" font-family="monospace" fill="#64748b">  href="foto.jpg"</text>
  <text x="160" y="92" font-size="7" font-family="monospace" fill="#64748b">  x="40" y="50"</text>
  <text x="160" y="104" font-size="7" font-family="monospace" fill="#64748b">  width="80"</text>
  <text x="160" y="116" font-size="7" font-family="monospace" fill="#64748b">  height="80"/&gt;</text>
  <text x="160" y="132" font-size="6" fill="#ef4444">width/height obligatorios</text>

  <text x="150" y="165" text-anchor="middle" font-size="7" fill="#94a3b8">
    sin width/height, la imagen no se renderiza
  </text>
</svg>
```

---

## Formatos admitidos

```svg
<svg width="300" height="200"
     viewBox="0 0 300 200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">formatos soportados</text>

  <rect x="15" y="34" width="270" height="38" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="48" font-size="8" font-weight="bold" fill="#1e40af">Rasterizados</text>
  <text x="25" y="60" font-size="7" fill="#64748b">PNG · JPEG · GIF · WebP · AVIF (moderno)</text>

  <rect x="15" y="78" width="270" height="38" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="92" font-size="8" font-weight="bold" fill="#166534">SVG como imagen</text>
  <text x="25" y="104" font-size="7" fill="#64748b">otro archivo .svg referenciado → imagen opaca</text>

  <rect x="15" y="122" width="270" height="38" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="136" font-size="8" font-weight="bold" fill="#92400e">Data URI</text>
  <text x="25" y="148" font-size="7" fill="#64748b">data:image/png;base64,... · embebido, sin HTTP</text>

  <text x="150" y="180" text-anchor="middle" font-size="7" fill="#94a3b8">
    el contenido de &lt;image&gt; es opaco: no se accede a su DOM
  </text>
  <text x="150" y="192" text-anchor="middle" font-size="7" fill="#94a3b8">
    ni los estilos del padre ni sus scripts lo afectan
  </text>
</svg>
```

---

## `preserveAspectRatio`: cómo se ajusta la imagen

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">imagen cuadrada en caja rectangular</text>

  <defs>
    <!-- Imagen cuadrada 80x80 dentro de una caja rectangular -->
    <symbol id="placeholder-img" viewBox="0 0 80 80">
      <rect width="80" height="80" fill="#8b5cf6"/>
      <circle cx="40" cy="40" r="26" fill="#fef9c3"/>
      <polygon points="40,20 52,58 22,34 58,34 28,58" fill="#8b5cf6"/>
    </symbol>
  </defs>

  <!-- meet (default) -->
  <rect x="15" y="40" width="85" height="55" fill="#f8fafc" stroke="#cbd5e1"/>
  <svg x="15" y="40" width="85" height="55" viewBox="0 0 80 80" preserveAspectRatio="xMidYMid meet">
    <use href="#placeholder-img"/>
  </svg>
  <text x="57" y="108" text-anchor="middle" font-size="7" fill="#64748b">meet</text>
  <text x="57" y="118" text-anchor="middle" font-size="6" fill="#94a3b8">cabe completa</text>

  <!-- slice -->
  <rect x="110" y="40" width="85" height="55" fill="#f8fafc" stroke="#cbd5e1"/>
  <svg x="110" y="40" width="85" height="55" viewBox="0 0 80 80" preserveAspectRatio="xMidYMid slice">
    <use href="#placeholder-img"/>
  </svg>
  <text x="152" y="108" text-anchor="middle" font-size="7" fill="#64748b">slice</text>
  <text x="152" y="118" text-anchor="middle" font-size="6" fill="#94a3b8">cubre y recorta</text>

  <!-- none -->
  <rect x="205" y="40" width="85" height="55" fill="#f8fafc" stroke="#cbd5e1"/>
  <svg x="205" y="40" width="85" height="55" viewBox="0 0 80 80" preserveAspectRatio="none">
    <use href="#placeholder-img"/>
  </svg>
  <text x="247" y="108" text-anchor="middle" font-size="7" fill="#64748b">none</text>
  <text x="247" y="118" text-anchor="middle" font-size="6" fill="#94a3b8">estira</text>

  <!-- Explicación -->
  <rect x="15" y="140" width="270" height="100" fill="#f1f5f9" stroke="#cbd5e1" rx="3"/>
  <text x="25" y="156" font-size="8" font-weight="bold" fill="#1e293b">sintaxis completa:</text>
  <text x="25" y="170" font-size="7" font-family="monospace" fill="#64748b">preserveAspectRatio="[align] [meet|slice|none]"</text>

  <text x="25" y="188" font-size="7" fill="#64748b">• align: xMidYMid (centro), xMinYMin (arriba-izq),</text>
  <text x="25" y="198" font-size="7" fill="#64748b">  xMaxYMax (abajo-der), xMidYMin, xMaxYMid, etc.</text>

  <text x="25" y="214" font-size="7" fill="#64748b">• meet: cabe completa, deja bandas vacías si hace falta</text>
  <text x="25" y="224" font-size="7" fill="#64748b">• slice: cubre toda el área, recorta el sobrante</text>
  <text x="25" y="234" font-size="7" fill="#64748b">• none: estira hasta llenar (ignora el aspect ratio)</text>
</svg>
```

---

## Imagen recortada con `<clipPath>`: avatar circular

```svg
<svg width="300" height="200"
     viewBox="0 0 300 200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">image + clipPath = avatar circular</text>

  <defs>
    <clipPath id="avatar-circle">
      <circle cx="225" cy="100" r="45"/>
    </clipPath>
  </defs>

  <!-- Imagen sin recorte -->
  <image href="data:image/svg+xml;utf8,&lt;svg xmlns='http://www.w3.org/2000/svg' width='100' height='100'&gt;&lt;defs&gt;&lt;linearGradient id='g' x1='0' y1='0' x2='1' y2='1'&gt;&lt;stop offset='0' stop-color='%23f59e0b'/&gt;&lt;stop offset='0.5' stop-color='%23ec4899'/&gt;&lt;stop offset='1' stop-color='%238b5cf6'/&gt;&lt;/linearGradient&gt;&lt;/defs&gt;&lt;rect width='100' height='100' fill='url(%23g)'/&gt;&lt;circle cx='50' cy='40' r='16' fill='white' opacity='0.8'/&gt;&lt;path d='M 20 85 Q 50 60 80 85' fill='white' opacity='0.8'/&gt;&lt;/svg&gt;"
         x="30" y="55" width="90" height="90"/>
  <text x="75" y="165" text-anchor="middle" font-size="7" fill="#64748b">imagen original</text>

  <!-- Imagen con clipPath circular -->
  <image href="data:image/svg+xml;utf8,&lt;svg xmlns='http://www.w3.org/2000/svg' width='100' height='100'&gt;&lt;defs&gt;&lt;linearGradient id='g' x1='0' y1='0' x2='1' y2='1'&gt;&lt;stop offset='0' stop-color='%23f59e0b'/&gt;&lt;stop offset='0.5' stop-color='%23ec4899'/&gt;&lt;stop offset='1' stop-color='%238b5cf6'/&gt;&lt;/linearGradient&gt;&lt;/defs&gt;&lt;rect width='100' height='100' fill='url(%23g)'/&gt;&lt;circle cx='50' cy='40' r='16' fill='white' opacity='0.8'/&gt;&lt;path d='M 20 85 Q 50 60 80 85' fill='white' opacity='0.8'/&gt;&lt;/svg&gt;"
         x="180" y="55" width="90" height="90"
         clip-path="url(#avatar-circle)"/>

  <!-- Anillo decorativo -->
  <circle cx="225" cy="100" r="45" fill="none" stroke="#1e293b" stroke-width="2"/>

  <text x="225" y="165" text-anchor="middle" font-size="7" fill="#64748b">con clip-path</text>

  <text x="150" y="190" text-anchor="middle" font-size="7" fill="#94a3b8">
    &lt;image&gt; acepta clip-path, mask y filter como cualquier otro elemento
  </text>
</svg>
```

---

## Imagen con filtro aplicado

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">filtros sobre &lt;image&gt;</text>

  <defs>
    <filter id="img-gray">
      <feColorMatrix type="saturate" values="0"/>
    </filter>
    <filter id="img-blur">
      <feGaussianBlur stdDeviation="2"/>
    </filter>
    <filter id="img-hue">
      <feColorMatrix type="hueRotate" values="180"/>
    </filter>
  </defs>

  <!-- Imagen base (data URI SVG) -->
  <image href="data:image/svg+xml;utf8,&lt;svg xmlns='http://www.w3.org/2000/svg' width='70' height='70'&gt;&lt;rect width='70' height='70' fill='%23f59e0b'/&gt;&lt;circle cx='35' cy='35' r='22' fill='%23ec4899'/&gt;&lt;polygon points='35,15 45,45 25,45' fill='%238b5cf6'/&gt;&lt;/svg&gt;"
         x="15" y="45" width="60" height="60"/>
  <text x="45" y="120" text-anchor="middle" font-size="7" fill="#64748b">original</text>

  <image href="data:image/svg+xml;utf8,&lt;svg xmlns='http://www.w3.org/2000/svg' width='70' height='70'&gt;&lt;rect width='70' height='70' fill='%23f59e0b'/&gt;&lt;circle cx='35' cy='35' r='22' fill='%23ec4899'/&gt;&lt;polygon points='35,15 45,45 25,45' fill='%238b5cf6'/&gt;&lt;/svg&gt;"
         x="85" y="45" width="60" height="60"
         filter="url(#img-gray)"/>
  <text x="115" y="120" text-anchor="middle" font-size="7" fill="#64748b">grises</text>

  <image href="data:image/svg+xml;utf8,&lt;svg xmlns='http://www.w3.org/2000/svg' width='70' height='70'&gt;&lt;rect width='70' height='70' fill='%23f59e0b'/&gt;&lt;circle cx='35' cy='35' r='22' fill='%23ec4899'/&gt;&lt;polygon points='35,15 45,45 25,45' fill='%238b5cf6'/&gt;&lt;/svg&gt;"
         x="155" y="45" width="60" height="60"
         filter="url(#img-blur)"/>
  <text x="185" y="120" text-anchor="middle" font-size="7" fill="#64748b">blur</text>

  <image href="data:image/svg+xml;utf8,&lt;svg xmlns='http://www.w3.org/2000/svg' width='70' height='70'&gt;&lt;rect width='70' height='70' fill='%23f59e0b'/&gt;&lt;circle cx='35' cy='35' r='22' fill='%23ec4899'/&gt;&lt;polygon points='35,15 45,45 25,45' fill='%238b5cf6'/&gt;&lt;/svg&gt;"
         x="225" y="45" width="60" height="60"
         filter="url(#img-hue)"/>
  <text x="255" y="120" text-anchor="middle" font-size="7" fill="#64748b">hue 180</text>

  <text x="150" y="160" text-anchor="middle" font-size="7" fill="#94a3b8">
    los filtros SVG funcionan sobre el bitmap de la imagen
  </text>
  <text x="150" y="178" text-anchor="middle" font-size="7" fill="#94a3b8">
    equivale a editar la foto sin alterar el archivo original
  </text>
  <text x="150" y="200" text-anchor="middle" font-size="6" fill="#ef4444">
    precaución: filtros sobre imágenes grandes son costosos
  </text>
</svg>
```

---

## `href` vs `xlink:href` — compatibilidad

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" fill="#f8fafc" font-weight="bold">
    href vs xlink:href
  </text>

  <rect x="10" y="36" width="280" height="52" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="18" y="50" font-size="8" font-weight="bold" fill="#166534">href (SVG 2 · moderno)</text>
  <text x="18" y="64" font-size="7" font-family="monospace" fill="#1e293b">&lt;image href="foto.jpg" .../&gt;</text>
  <text x="18" y="80" font-size="7" fill="#64748b">sintaxis recomendada · soporte universal desde 2017</text>

  <rect x="10" y="98" width="280" height="52" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="18" y="112" font-size="8" font-weight="bold" fill="#92400e">xlink:href (SVG 1.1 · legacy)</text>
  <text x="18" y="126" font-size="7" font-family="monospace" fill="#1e293b">&lt;image xlink:href="foto.jpg" .../&gt;</text>
  <text x="18" y="142" font-size="7" fill="#64748b">requiere xmlns:xlink en el &lt;svg&gt; raíz</text>

  <rect x="10" y="160" width="280" height="48" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="18" y="174" font-size="8" font-weight="bold" fill="#1e40af">máxima compatibilidad</text>
  <text x="18" y="188" font-size="7" font-family="monospace" fill="#1e293b">&lt;image href="..." xlink:href="..." .../&gt;</text>
  <text x="18" y="202" font-size="7" fill="#64748b">incluir ambos para navegadores muy antiguos</text>
</svg>
```
