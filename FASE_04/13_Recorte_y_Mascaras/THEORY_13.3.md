# 13.3 Máscaras con `<mask>`

`<mask>` controla la visibilidad de cada píxel usando la **luminancia** del contenido. A diferencia de `clipPath`, permite semitransparencias: blanco = visible, negro = oculto, gris = parcial.

---

## La regla de luminancia

```svg
<svg width="300" height="180"
     viewBox="0 0 300 180"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">blanco = visible · negro = oculto</text>

  <defs>
    <mask id="m-blanco">
      <rect x="0" y="0" width="300" height="200" fill="white"/>
    </mask>
    <mask id="m-negro">
      <rect x="0" y="0" width="300" height="200" fill="black"/>
    </mask>
    <mask id="m-gris">
      <rect x="0" y="0" width="300" height="200" fill="#808080"/>
    </mask>
  </defs>

  <!-- Tres rectángulos idénticos, diferente máscara -->
  <rect x="20" y="40" width="70" height="70" fill="#3b82f6" mask="url(#m-blanco)"/>
  <text x="55" y="125" text-anchor="middle" font-size="8" fill="#64748b">mask: blanco</text>
  <text x="55" y="138" text-anchor="middle" font-size="7" fill="#10b981">100% visible</text>

  <rect x="115" y="40" width="70" height="70" fill="#3b82f6" mask="url(#m-gris)"/>
  <text x="150" y="125" text-anchor="middle" font-size="8" fill="#64748b">mask: gris 50%</text>
  <text x="150" y="138" text-anchor="middle" font-size="7" fill="#f59e0b">50% visible</text>

  <rect x="210" y="40" width="70" height="70" fill="#3b82f6" mask="url(#m-negro)"/>
  <text x="245" y="125" text-anchor="middle" font-size="8" fill="#64748b">mask: negro</text>
  <text x="245" y="138" text-anchor="middle" font-size="7" fill="#ef4444">0% visible</text>

  <text x="150" y="165" text-anchor="middle" font-size="8" fill="#94a3b8">
    la luminancia de la máscara controla la opacidad del resultado
  </text>
</svg>
```

---

## Máscara con forma: blanco sobre negro

```svg
<svg width="300" height="170"
     viewBox="0 0 300 170"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">fondo negro + forma blanca = recorte</text>

  <defs>
    <mask id="m-circulo">
      <!-- Fondo negro: todo oculto por defecto -->
      <rect x="0" y="0" width="300" height="200" fill="black"/>
      <!-- Forma blanca: visible solo dentro del círculo -->
      <circle cx="75" cy="80" r="35" fill="white"/>
    </mask>
  </defs>

  <!-- Visualización de la propia máscara -->
  <rect x="20" y="35" width="110" height="95" fill="#1e293b"/>
  <circle cx="75" cy="80" r="35" fill="white"/>
  <text x="75" y="145" text-anchor="middle" font-size="7" fill="#64748b">la máscara</text>

  <!-- Flecha visual -->
  <text x="155" y="85" text-anchor="middle" font-size="14">→</text>

  <!-- Resultado aplicado -->
  <rect x="180" y="35" width="110" height="95" fill="#3b82f6" mask="url(#m-circulo)"/>
  <text x="235" y="145" text-anchor="middle" font-size="7" fill="#64748b">resultado</text>

  <text x="150" y="160" text-anchor="middle" font-size="8" fill="#94a3b8">
    equivale a un clipPath circular
  </text>
</svg>
```

---

## El fondo por defecto ES negro: añadir blanco explícito

```svg
<svg width="300" height="180"
     viewBox="0 0 300 180"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">sin rect blanco = todo invisible</text>

  <defs>
    <!-- MAL: solo un círculo negro, sin fondo blanco -->
    <mask id="m-mal">
      <circle cx="55" cy="80" r="25" fill="black"/>
    </mask>
    <!-- BIEN: rect blanco de fondo + círculo negro como agujero -->
    <mask id="m-bien">
      <rect x="0" y="0" width="300" height="200" fill="white"/>
      <circle cx="235" cy="80" r="25" fill="black"/>
    </mask>
  </defs>

  <!-- Caso mal -->
  <rect x="15" y="35" width="130" height="100" fill="#fef2f2" stroke="#ef4444" stroke-width="1.5" rx="4"/>
  <text x="80" y="52" text-anchor="middle" font-size="9" fill="#9d174d" font-weight="bold">sin fondo blanco</text>
  <!-- Este rect no se ve porque la máscara es casi toda negra -->
  <rect x="30" y="60" width="100" height="60" fill="#ef4444" mask="url(#m-mal)"/>
  <text x="80" y="152" text-anchor="middle" font-size="7" fill="#ef4444">todo el rect desapareció</text>

  <!-- Caso bien -->
  <rect x="155" y="35" width="130" height="100" fill="#dcfce7" stroke="#10b981" stroke-width="1.5" rx="4"/>
  <text x="220" y="52" text-anchor="middle" font-size="9" fill="#166534" font-weight="bold">con fondo blanco</text>
  <rect x="170" y="60" width="100" height="60" fill="#10b981" mask="url(#m-bien)"/>
  <text x="220" y="152" text-anchor="middle" font-size="7" fill="#166534">agujero circular visible</text>

  <text x="150" y="170" text-anchor="middle" font-size="7" fill="#94a3b8">
    siempre empezar con un rect blanco + añadir negros donde haya agujeros
  </text>
</svg>
```

---

## Gradiente en la máscara: desvanecimiento suave

```svg
<svg width="300" height="170"
     viewBox="0 0 300 170"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">el poder de las máscaras: gradientes</text>

  <defs>
    <linearGradient id="grad-mask" x1="0" y1="0" x2="1" y2="0">
      <stop offset="0"   stop-color="white"/>
      <stop offset="1"   stop-color="black"/>
    </linearGradient>

    <mask id="m-fade">
      <rect x="0" y="0" width="300" height="200" fill="url(#grad-mask)"/>
    </mask>

    <linearGradient id="color-arco" x1="0" y1="0" x2="1" y2="0">
      <stop offset="0"   stop-color="#ec4899"/>
      <stop offset="0.5" stop-color="#8b5cf6"/>
      <stop offset="1"   stop-color="#3b82f6"/>
    </linearGradient>
  </defs>

  <!-- Fondo de tablero de ajedrez para ver la transparencia -->
  <defs>
    <pattern id="ajedrez" width="10" height="10" patternUnits="userSpaceOnUse">
      <rect width="10" height="10" fill="#f1f5f9"/>
      <rect width="5" height="5" fill="#cbd5e1"/>
      <rect x="5" y="5" width="5" height="5" fill="#cbd5e1"/>
    </pattern>
  </defs>
  <rect x="20" y="40" width="260" height="75" fill="url(#ajedrez)"/>

  <!-- El rectángulo con la máscara de gradiente: se desvanece de izquierda a derecha -->
  <rect x="20" y="40" width="260" height="75" fill="url(#color-arco)" mask="url(#m-fade)"/>

  <text x="150" y="140" text-anchor="middle" font-size="8" fill="#94a3b8">
    clipPath no puede hacer esto: necesita mask
  </text>
</svg>
```

---

## Máscara circular con viñeta suave

```svg
<svg width="300" height="170"
     viewBox="0 0 300 170"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">viñeta radial con máscara</text>

  <defs>
    <radialGradient id="grad-vineta" cx="0.5" cy="0.5" r="0.5">
      <stop offset="0"   stop-color="white"/>
      <stop offset="0.7" stop-color="white"/>
      <stop offset="1"   stop-color="black"/>
    </radialGradient>

    <mask id="m-vineta">
      <rect x="0" y="0" width="300" height="200" fill="url(#grad-vineta)"/>
    </mask>

    <!-- Un "paisaje" simple de colores -->
    <linearGradient id="paisaje" x1="0" y1="0" x2="0" y2="1">
      <stop offset="0"   stop-color="#f59e0b"/>
      <stop offset="0.5" stop-color="#ec4899"/>
      <stop offset="1"   stop-color="#4c1d95"/>
    </linearGradient>
  </defs>

  <rect x="0" y="35" width="300" height="110" fill="url(#paisaje)" mask="url(#m-vineta)"/>

  <text x="150" y="160" text-anchor="middle" font-size="8" fill="#94a3b8">
    bordes difuminados — imposible con clipPath
  </text>
</svg>
```

---

## Texto con fade de izquierda a derecha

```svg
<svg width="300" height="140"
     viewBox="0 0 300 140"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">texto que se desvanece</text>

  <defs>
    <linearGradient id="fade-txt" x1="0" y1="0" x2="1" y2="0">
      <stop offset="0.3" stop-color="white"/>
      <stop offset="1"   stop-color="black"/>
    </linearGradient>

    <mask id="m-txt-fade">
      <rect x="0" y="0" width="300" height="140" fill="url(#fade-txt)"/>
    </mask>
  </defs>

  <!-- Texto normal -->
  <text x="25" y="60" font-size="22" font-weight="bold" fill="#1e293b" font-family="sans-serif">sin máscara</text>

  <!-- Texto con máscara: lo mismo pero con fade a la derecha -->
  <g mask="url(#m-txt-fade)">
    <text x="25" y="105" font-size="22" font-weight="bold" fill="#1e293b" font-family="sans-serif">con máscara fade</text>
  </g>

  <text x="150" y="130" text-anchor="middle" font-size="7" fill="#94a3b8">
    usado en menús horizontales con overflow, etiquetas truncadas, etc.
  </text>
</svg>
```
