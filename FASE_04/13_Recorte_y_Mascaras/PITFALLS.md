# Errores comunes — Recorte y Máscaras

---

## 1. Olvidar el fondo blanco de la máscara

Lo que no está cubierto por el contenido de una `<mask>` es **negro**, es decir, invisible. Si añades solo una forma negra esperando "abrir un agujero", lo que haces en realidad es ocultar todo el elemento.

```svg
<svg width="300" height="190"
     viewBox="0 0 300 190"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <!-- MAL: solo un círculo negro -->
    <mask id="mask-mal-fondo">
      <circle cx="55" cy="90" r="25" fill="black"/>
    </mask>
    <!-- BIEN: rect blanco de base + círculo negro -->
    <mask id="mask-bien-fondo">
      <rect x="0" y="0" width="300" height="200" fill="white"/>
      <circle cx="225" cy="90" r="25" fill="black"/>
    </mask>
  </defs>

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">el fondo de una mask es negro</text>

  <!-- MAL -->
  <rect x="15" y="35" width="130" height="140" fill="#fef2f2" stroke="#ef4444" stroke-width="1.5" rx="4"/>
  <text x="80" y="52" text-anchor="middle" font-size="9" fill="#9d174d" font-weight="bold">MAL</text>
  <text x="80" y="66" text-anchor="middle" font-size="7" fill="#64748b">solo circle black</text>
  <rect x="30" y="72" width="100" height="80" fill="#ef4444" mask="url(#mask-mal-fondo)"/>
  <text x="80" y="165" text-anchor="middle" font-size="7" fill="#ef4444">el rect se hizo invisible</text>

  <!-- BIEN -->
  <rect x="155" y="35" width="130" height="140" fill="#dcfce7" stroke="#10b981" stroke-width="1.5" rx="4"/>
  <text x="220" y="52" text-anchor="middle" font-size="9" fill="#166534" font-weight="bold">BIEN</text>
  <text x="220" y="66" text-anchor="middle" font-size="7" fill="#64748b">rect white + circle black</text>
  <rect x="170" y="72" width="100" height="80" fill="#10b981" mask="url(#mask-bien-fondo)"/>
  <text x="220" y="165" text-anchor="middle" font-size="7" fill="#166534">agujero visible</text>
</svg>
```

---

## 2. Usar `<clipPath>` cuando el recorte necesita un gradiente

`<clipPath>` es binario: no hace desvanecimientos. Si intentas usar un `fill="url(#grad)"` en un elemento dentro de un `clipPath`, el gradiente se ignora — solo cuenta la geometría. Para un borde difuminado, necesitas `<mask>`.

```svg
<svg width="300" height="190"
     viewBox="0 0 300 190"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <!-- MAL: clipPath con "gradient fill" — el gradiente se ignora -->
    <linearGradient id="grad-fade" x1="0" y1="0" x2="1" y2="0">
      <stop offset="0" stop-color="white"/>
      <stop offset="1" stop-color="black"/>
    </linearGradient>
    <clipPath id="cp-intento-fade">
      <rect x="0" y="0" width="100" height="80" fill="url(#grad-fade)"/>
    </clipPath>
    <!-- BIEN: mask con el mismo gradiente -->
    <mask id="mask-fade-bien">
      <rect x="0" y="0" width="300" height="200" fill="url(#grad-fade)"/>
    </mask>
  </defs>

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">clipPath ignora gradientes</text>

  <rect x="15" y="35" width="130" height="140" fill="#fef2f2" stroke="#ef4444" stroke-width="1.5" rx="4"/>
  <text x="80" y="52" text-anchor="middle" font-size="9" fill="#9d174d" font-weight="bold">MAL</text>
  <text x="80" y="66" text-anchor="middle" font-size="7" fill="#64748b">clipPath + gradient fill</text>
  <rect x="30" y="75" width="100" height="80" fill="#ef4444" clip-path="url(#cp-intento-fade)"/>
  <text x="80" y="165" text-anchor="middle" font-size="7" fill="#ef4444">borde nítido, sin fade</text>

  <rect x="155" y="35" width="130" height="140" fill="#dcfce7" stroke="#10b981" stroke-width="1.5" rx="4"/>
  <text x="220" y="52" text-anchor="middle" font-size="9" fill="#166534" font-weight="bold">BIEN</text>
  <text x="220" y="66" text-anchor="middle" font-size="7" fill="#64748b">mask con gradiente</text>
  <rect x="170" y="75" width="100" height="80" fill="#10b981" mask="url(#mask-fade-bien)"/>
  <text x="220" y="165" text-anchor="middle" font-size="7" fill="#166534">desvanecimiento real</text>
</svg>
```

---

## 3. Asumir que `objectBoundingBox` mantiene proporciones

Cuando usas `clipPathUnits="objectBoundingBox"` con un círculo en `r="0.5"`, el resultado en un elemento rectangular es una elipse, no un círculo. Las coordenadas 0-1 se mapean a las proporciones del bbox, que rara vez es cuadrado.

```svg
<svg width="300" height="180"
     viewBox="0 0 300 180"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <clipPath id="bbox-redondo" clipPathUnits="objectBoundingBox">
      <circle cx="0.5" cy="0.5" r="0.45"/>
    </clipPath>
  </defs>

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">bbox deforma en rectángulos</text>

  <!-- Esperado vs obtenido -->
  <rect x="15" y="35" width="130" height="130" fill="#fef2f2" stroke="#ef4444" stroke-width="1.5" rx="4"/>
  <text x="80" y="52" text-anchor="middle" font-size="9" fill="#9d174d" font-weight="bold">SORPRESA</text>
  <text x="80" y="64" text-anchor="middle" font-size="7" fill="#64748b">rect ancho + círculo bbox</text>
  <rect x="25" y="80" width="110" height="50" fill="#ef4444" clip-path="url(#bbox-redondo)"/>
  <text x="80" y="148" text-anchor="middle" font-size="7" fill="#ef4444">salió elipse, no círculo</text>

  <!-- Solución -->
  <rect x="155" y="35" width="130" height="130" fill="#dcfce7" stroke="#10b981" stroke-width="1.5" rx="4"/>
  <text x="220" y="52" text-anchor="middle" font-size="9" fill="#166534" font-weight="bold">SOLUCIÓN</text>
  <text x="220" y="64" text-anchor="middle" font-size="7" fill="#64748b">usar userSpaceOnUse</text>
  <defs>
    <clipPath id="user-redondo" clipPathUnits="userSpaceOnUse">
      <circle cx="220" cy="105" r="25"/>
    </clipPath>
  </defs>
  <rect x="165" y="80" width="110" height="50" fill="#10b981" clip-path="url(#user-redondo)"/>
  <text x="220" y="148" text-anchor="middle" font-size="7" fill="#166534">círculo real (coordenadas abs)</text>
</svg>
```

---

## 4. Esperar intersección al poner varias formas en un clipPath

Cuando un `<clipPath>` contiene varias formas, el resultado es la **unión** de todas, no la intersección. Si querías mostrar solo la zona común a dos formas, necesitas anidar clipPaths o usar un path único con operaciones booleanas previas.

```svg
<svg width="300" height="190"
     viewBox="0 0 300 190"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <!-- MAL: dos círculos en un clipPath → se unen -->
    <clipPath id="cp-union">
      <circle cx="55" cy="95" r="30"/>
      <circle cx="85" cy="95" r="30"/>
    </clipPath>

    <!-- BIEN: clipPath anidado para lograr intersección -->
    <clipPath id="cp-outer">
      <circle cx="195" cy="95" r="30"/>
    </clipPath>
    <clipPath id="cp-inner">
      <circle cx="225" cy="95" r="30"/>
    </clipPath>
  </defs>

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">varias formas = UNIÓN, no intersección</text>

  <!-- Unión: los dos círculos juntos -->
  <rect x="15" y="35" width="130" height="140" fill="#fef2f2" stroke="#ef4444" stroke-width="1.5" rx="4"/>
  <text x="80" y="52" text-anchor="middle" font-size="9" fill="#9d174d" font-weight="bold">MAL</text>
  <text x="80" y="66" text-anchor="middle" font-size="7" fill="#64748b">intenta intersección</text>
  <rect x="20" y="60" width="120" height="90" fill="#ef4444" clip-path="url(#cp-union)"/>
  <text x="80" y="165" text-anchor="middle" font-size="7" fill="#ef4444">resultado: unión</text>

  <!-- Intersección: clipPath anidado -->
  <rect x="155" y="35" width="130" height="140" fill="#dcfce7" stroke="#10b981" stroke-width="1.5" rx="4"/>
  <text x="220" y="52" text-anchor="middle" font-size="9" fill="#166534" font-weight="bold">BIEN</text>
  <text x="220" y="66" text-anchor="middle" font-size="7" fill="#64748b">clipPath anidado</text>
  <g clip-path="url(#cp-outer)">
    <rect x="160" y="60" width="120" height="90" fill="#10b981" clip-path="url(#cp-inner)"/>
  </g>
  <text x="220" y="165" text-anchor="middle" font-size="7" fill="#166534">resultado: intersección</text>
</svg>
```

---

## 5. Aplicar `clip-path` o `mask` a un elemento que "no se mueve con él"

Si el `clipPath` usa `userSpaceOnUse` (el valor por defecto) y luego mueves el elemento con `transform`, el clip se queda en su posición original. El elemento puede salirse del recorte sin querer.

```svg
<svg width="300" height="190"
     viewBox="0 0 300 190"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <clipPath id="cp-fijo">
      <circle cx="75" cy="95" r="30"/>
    </clipPath>
    <clipPath id="cp-relativo" clipPathUnits="objectBoundingBox">
      <circle cx="0.5" cy="0.5" r="0.45"/>
    </clipPath>
  </defs>

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">clip fijo vs clip que sigue al elemento</text>

  <!-- MAL: userSpaceOnUse + transform → el clip no acompaña -->
  <rect x="15" y="35" width="130" height="140" fill="#fef2f2" stroke="#ef4444" stroke-width="1.5" rx="4"/>
  <text x="80" y="52" text-anchor="middle" font-size="9" fill="#9d174d" font-weight="bold">MAL</text>
  <text x="80" y="66" text-anchor="middle" font-size="7" fill="#64748b">userSpaceOnUse + translate</text>
  <!-- La referencia del clip queda en cx=75,cy=95 pero el rect va trasladado -->
  <circle cx="75" cy="95" r="30" fill="none" stroke="#ef4444" stroke-dasharray="2,2"/>
  <rect x="20" y="75" width="60" height="60" fill="#ef4444"
        clip-path="url(#cp-fijo)"
        transform="translate(40, 0)"/>
  <text x="80" y="165" text-anchor="middle" font-size="7" fill="#ef4444">el clip se quedó atrás</text>

  <!-- BIEN: objectBoundingBox → el clip acompaña -->
  <rect x="155" y="35" width="130" height="140" fill="#dcfce7" stroke="#10b981" stroke-width="1.5" rx="4"/>
  <text x="220" y="52" text-anchor="middle" font-size="9" fill="#166534" font-weight="bold">BIEN</text>
  <text x="220" y="66" text-anchor="middle" font-size="7" fill="#64748b">objectBoundingBox</text>
  <rect x="160" y="75" width="60" height="60" fill="#10b981"
        clip-path="url(#cp-relativo)"
        transform="translate(40, 0)"/>
  <text x="220" y="165" text-anchor="middle" font-size="7" fill="#166534">el clip sigue al elemento</text>
</svg>
```
