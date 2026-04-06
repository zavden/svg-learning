# 13.4 Atributos de `<mask>`

Una máscara tiene más parámetros que un `<clipPath>`: define su propia área (`x`, `y`, `width`, `height`), su sistema de unidades (`maskUnits`) y el sistema de coordenadas del contenido (`maskContentUnits`).

---

## `x`, `y`, `width`, `height` — el área activa

```svg
<svg width="300" height="170"
     viewBox="0 0 300 170"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">el área de la máscara</text>

  <defs>
    <!-- Defaults: -10% -10% 120% 120% (un poco más grande que el elemento) -->
    <mask id="m-default">
      <rect x="-10%" y="-10%" width="120%" height="120%" fill="white"/>
      <circle cx="50%" cy="50%" r="25%" fill="black"/>
    </mask>

    <!-- Área reducida: solo cubre la mitad del rect -->
    <mask id="m-reducida" x="0" y="0" width="0.5" height="1">
      <rect x="0" y="0" width="100%" height="100%" fill="white"/>
      <circle cx="50%" cy="50%" r="25%" fill="black"/>
    </mask>
  </defs>

  <!-- Máscara con área por defecto: cubre todo el rect -->
  <rect x="25" y="45" width="100" height="80" fill="#3b82f6" mask="url(#m-default)"/>
  <text x="75"  y="140" text-anchor="middle" font-size="7" fill="#64748b">área por defecto</text>
  <text x="75"  y="152" text-anchor="middle" font-size="6" fill="#94a3b8">-10% -10% 120% 120%</text>

  <!-- Máscara con área reducida: la parte fuera del área queda negra (invisible) -->
  <rect x="170" y="45" width="100" height="80" fill="#3b82f6" mask="url(#m-reducida)"/>
  <text x="220" y="140" text-anchor="middle" font-size="7" fill="#64748b">área 0 0 0.5 1</text>
  <text x="220" y="152" text-anchor="middle" font-size="6" fill="#ef4444">mitad derecha queda fuera</text>
</svg>
```

---

## `maskUnits` — unidades del área de la máscara

```svg
<svg width="300" height="180"
     viewBox="0 0 300 180"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">maskUnits = sistema del área</text>

  <defs>
    <!-- objectBoundingBox (por defecto): x, y, width, height son fracciones 0-1 -->
    <mask id="m-obj" maskUnits="objectBoundingBox" x="0" y="0" width="1" height="1">
      <rect x="0" y="0" width="100%" height="100%" fill="white"/>
      <rect x="25%" y="25%" width="50%" height="50%" fill="black"/>
    </mask>

    <!-- userSpaceOnUse: x, y, width, height en coordenadas absolutas -->
    <mask id="m-usr" maskUnits="userSpaceOnUse" x="170" y="45" width="100" height="80">
      <rect x="170" y="45" width="100" height="80" fill="white"/>
      <rect x="195" y="65" width="50"  height="40" fill="black"/>
    </mask>
  </defs>

  <rect x="25" y="45" width="100" height="80" fill="#8b5cf6" mask="url(#m-obj)"/>
  <text x="75" y="140" text-anchor="middle" font-size="7" fill="#64748b">maskUnits=</text>
  <text x="75" y="152" text-anchor="middle" font-size="7" fill="#64748b">objectBoundingBox</text>
  <text x="75" y="165" text-anchor="middle" font-size="6" fill="#94a3b8">valor por defecto</text>

  <rect x="170" y="45" width="100" height="80" fill="#8b5cf6" mask="url(#m-usr)"/>
  <text x="220" y="140" text-anchor="middle" font-size="7" fill="#64748b">maskUnits=</text>
  <text x="220" y="152" text-anchor="middle" font-size="7" fill="#64748b">userSpaceOnUse</text>
  <text x="220" y="165" text-anchor="middle" font-size="6" fill="#94a3b8">coordenadas absolutas</text>
</svg>
```

---

## `maskContentUnits` — unidades del contenido

```svg
<svg width="300" height="180"
     viewBox="0 0 300 180"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">maskContentUnits = sistema del contenido</text>

  <defs>
    <!-- Contenido con coordenadas absolutas (por defecto) -->
    <mask id="m-content-user">
      <rect x="0" y="0" width="100%" height="100%" fill="white"/>
      <circle cx="75" cy="85" r="30" fill="black"/>
    </mask>

    <!-- Contenido con coordenadas relativas al bounding box: 0-1 -->
    <mask id="m-content-bbox" maskContentUnits="objectBoundingBox">
      <rect x="0" y="0" width="1" height="1" fill="white"/>
      <circle cx="0.5" cy="0.5" r="0.3" fill="black"/>
    </mask>
  </defs>

  <!-- userSpaceOnUse: el círculo está en 75,85 absoluto — solo coincide con un rect -->
  <rect x="25" y="45" width="100" height="80" fill="#10b981" mask="url(#m-content-user)"/>
  <text x="75" y="140" text-anchor="middle" font-size="7" fill="#64748b">contentUnits=</text>
  <text x="75" y="152" text-anchor="middle" font-size="7" fill="#64748b">userSpaceOnUse</text>
  <text x="75" y="165" text-anchor="middle" font-size="6" fill="#94a3b8">cx=75 cy=85 absolutos</text>

  <!-- objectBoundingBox: el círculo siempre en el centro relativo del elemento -->
  <rect x="170" y="45" width="100" height="80" fill="#10b981" mask="url(#m-content-bbox)"/>
  <text x="220" y="140" text-anchor="middle" font-size="7" fill="#64748b">contentUnits=</text>
  <text x="220" y="152" text-anchor="middle" font-size="7" fill="#64748b">objectBoundingBox</text>
  <text x="220" y="165" text-anchor="middle" font-size="6" fill="#94a3b8">cx=0.5 cy=0.5 relativos</text>
</svg>
```

---

## Cuatro combinaciones de `maskUnits` + `maskContentUnits`

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="10" fill="#f8fafc" font-weight="bold">
    maskUnits × maskContentUnits
  </text>

  <!-- Encabezado tabla -->
  <rect x="10" y="34" width="280" height="20" fill="#e2e8f0"/>
  <line x1="100" y1="34" x2="100" y2="54" stroke="#94a3b8" stroke-width="1"/>
  <line x1="195" y1="34" x2="195" y2="54" stroke="#94a3b8" stroke-width="1"/>
  <text x="55"  y="48" text-anchor="middle" font-size="8" font-weight="bold" fill="#1e293b">maskUnits</text>
  <text x="147" y="44" text-anchor="middle" font-size="7" font-weight="bold" fill="#1e293b">contentUnits</text>
  <text x="147" y="52" text-anchor="middle" font-size="7" font-weight="bold" fill="#1e293b">userSpaceOnUse</text>
  <text x="242" y="44" text-anchor="middle" font-size="7" font-weight="bold" fill="#1e293b">contentUnits</text>
  <text x="242" y="52" text-anchor="middle" font-size="7" font-weight="bold" fill="#1e293b">objectBoundingBox</text>

  <!-- Fila 1: objectBoundingBox + userSpaceOnUse -->
  <rect x="10" y="58" width="280" height="70" fill="none" stroke="#e2e8f0" stroke-width="1"/>
  <line x1="100" y1="58" x2="100" y2="128" stroke="#e2e8f0"/>
  <line x1="195" y1="58" x2="195" y2="128" stroke="#e2e8f0"/>

  <text x="55" y="80" text-anchor="middle" font-size="7" fill="#1e293b">objectBBox</text>
  <text x="55" y="92" text-anchor="middle" font-size="6" fill="#64748b">(por defecto)</text>
  <text x="55" y="108" text-anchor="middle" font-size="6" fill="#64748b">área: 0-1</text>

  <text x="147" y="80" text-anchor="middle" font-size="6" fill="#64748b">área relativa</text>
  <text x="147" y="90" text-anchor="middle" font-size="6" fill="#64748b">contenido abs.</text>
  <text x="147" y="108" text-anchor="middle" font-size="6" fill="#ef4444">mezcla confusa</text>
  <text x="147" y="118" text-anchor="middle" font-size="6" fill="#ef4444">difícil de usar</text>

  <text x="242" y="80" text-anchor="middle" font-size="6" fill="#64748b">todo relativo</text>
  <text x="242" y="90" text-anchor="middle" font-size="6" fill="#64748b">al bounding box</text>
  <text x="242" y="108" text-anchor="middle" font-size="6" fill="#10b981">reutilizable</text>
  <text x="242" y="118" text-anchor="middle" font-size="6" fill="#10b981">✓ típico</text>

  <!-- Fila 2: userSpaceOnUse -->
  <rect x="10" y="132" width="280" height="70" fill="none" stroke="#e2e8f0" stroke-width="1"/>
  <line x1="100" y1="132" x2="100" y2="202" stroke="#e2e8f0"/>
  <line x1="195" y1="132" x2="195" y2="202" stroke="#e2e8f0"/>

  <text x="55" y="158" text-anchor="middle" font-size="7" fill="#1e293b">userSpaceOnUse</text>
  <text x="55" y="172" text-anchor="middle" font-size="6" fill="#64748b">área: coords</text>
  <text x="55" y="182" text-anchor="middle" font-size="6" fill="#64748b">absolutas</text>

  <text x="147" y="158" text-anchor="middle" font-size="6" fill="#64748b">todo absoluto</text>
  <text x="147" y="168" text-anchor="middle" font-size="6" fill="#64748b">en el SVG</text>
  <text x="147" y="182" text-anchor="middle" font-size="6" fill="#10b981">✓ máscara</text>
  <text x="147" y="192" text-anchor="middle" font-size="6" fill="#10b981">de posición fija</text>

  <text x="242" y="158" text-anchor="middle" font-size="6" fill="#64748b">área absoluta</text>
  <text x="242" y="168" text-anchor="middle" font-size="6" fill="#64748b">contenido relat.</text>
  <text x="242" y="182" text-anchor="middle" font-size="6" fill="#ef4444">raramente útil</text>

  <text x="150" y="215" text-anchor="middle" font-size="7" fill="#94a3b8">
    en la práctica: usa los defaults o todo userSpaceOnUse
  </text>
</svg>
```

---

## Cuándo ajustar el área: efectos que se desbordan

```svg
<svg width="300" height="180"
     viewBox="0 0 300 180"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">cuándo agrandar el área</text>

  <defs>
    <filter id="f-blur"><feGaussianBlur stdDeviation="6"/></filter>

    <!-- Máscara con blur en el contenido: el blur se extiende fuera del círculo -->
    <!-- Con área por defecto (-10% a 110%) el blur queda recortado -->
    <mask id="m-area-chica" x="0" y="0" width="1" height="1">
      <rect x="0" y="0" width="100%" height="100%" fill="black"/>
      <circle cx="50%" cy="50%" r="25%" fill="white" filter="url(#f-blur)"/>
    </mask>

    <!-- Área más grande para que el blur quepa -->
    <mask id="m-area-grande" x="-50%" y="-50%" width="200%" height="200%">
      <rect x="-50%" y="-50%" width="200%" height="200%" fill="black"/>
      <circle cx="50%" cy="50%" r="25%" fill="white" filter="url(#f-blur)"/>
    </mask>
  </defs>

  <!-- Área pequeña: el blur se recorta -->
  <rect x="25" y="45" width="100" height="80" fill="#ec4899" mask="url(#m-area-chica)"/>
  <rect x="25" y="45" width="100" height="80" fill="none" stroke="#ef4444" stroke-dasharray="2,2"/>
  <text x="75" y="140" text-anchor="middle" font-size="7" fill="#64748b">área = bbox</text>
  <text x="75" y="152" text-anchor="middle" font-size="7" fill="#ef4444">blur recortado</text>

  <!-- Área grande: el blur entra cómodo -->
  <rect x="170" y="45" width="100" height="80" fill="#ec4899" mask="url(#m-area-grande)"/>
  <rect x="170" y="45" width="100" height="80" fill="none" stroke="#94a3b8" stroke-dasharray="2,2"/>
  <text x="220" y="140" text-anchor="middle" font-size="7" fill="#64748b">área = 200%</text>
  <text x="220" y="152" text-anchor="middle" font-size="7" fill="#10b981">blur completo</text>

  <text x="150" y="170" text-anchor="middle" font-size="7" fill="#94a3b8">
    si la máscara tiene blur u otros efectos, ampliar el área
  </text>
</svg>
```
