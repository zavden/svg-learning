# Errores comunes — Filtros SVG

---

## 1. El área del filtro recorta el efecto

Por defecto un `<filter>` tiene `x="-10%" y="-10%" width="120%" height="120%"`. Un blur o una sombra fuerte se sale de ese margen y queda **cortada**. Síntoma típico: "mi sombra se ve como un cuadrado" o "el blur tiene un borde recto".

```svg
<svg width="300" height="190"
     viewBox="0 0 300 190"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <!-- MAL: área por defecto → la sombra grande se recorta -->
    <filter id="f-cortado">
      <feDropShadow dx="10" dy="12" stdDeviation="8" flood-opacity="0.5"/>
    </filter>
    <!-- BIEN: área generosa -->
    <filter id="f-amplio" x="-80%" y="-80%" width="260%" height="260%">
      <feDropShadow dx="10" dy="12" stdDeviation="8" flood-opacity="0.5"/>
    </filter>
  </defs>

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">el filter region recorta efectos</text>

  <!-- MAL -->
  <rect x="15" y="35" width="130" height="140" fill="#fef2f2" stroke="#ef4444" stroke-width="1.5" rx="4"/>
  <text x="80" y="52" text-anchor="middle" font-size="9" fill="#9d174d" font-weight="bold">MAL</text>
  <text x="80" y="66" text-anchor="middle" font-size="7" fill="#64748b">área por defecto</text>
  <rect x="50" y="80" width="60" height="60" rx="6" fill="#3b82f6" filter="url(#f-cortado)"/>
  <text x="80" y="162" text-anchor="middle" font-size="7" fill="#ef4444">sombra cortada en un borde recto</text>

  <!-- BIEN -->
  <rect x="155" y="35" width="130" height="140" fill="#dcfce7" stroke="#10b981" stroke-width="1.5" rx="4"/>
  <text x="220" y="52" text-anchor="middle" font-size="9" fill="#166534" font-weight="bold">BIEN</text>
  <text x="220" y="66" text-anchor="middle" font-size="7" fill="#64748b">área ampliada</text>
  <rect x="190" y="80" width="60" height="60" rx="6" fill="#3b82f6" filter="url(#f-amplio)"/>
  <text x="220" y="162" text-anchor="middle" font-size="7" fill="#166534">sombra completa y difusa</text>
</svg>
```

---

## 2. Usar `SourceGraphic` como base de una sombra

El resultado parece correcto a simple vista, pero la sombra **hereda los colores del elemento**. Cuando el elemento tiene colores vivos, la sombra sale verdosa, rosa o azulada en lugar de neutra. La solución es usar `SourceAlpha` (la silueta sin color) como entrada del blur.

```svg
<svg width="300" height="200"
     viewBox="0 0 300 200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <filter id="mal-srcgraphic" x="-50%" y="-50%" width="200%" height="200%">
      <feGaussianBlur in="SourceGraphic" stdDeviation="4" result="b"/>
      <feOffset in="b" dx="5" dy="6" result="o"/>
      <feMerge>
        <feMergeNode in="o"/>
        <feMergeNode in="SourceGraphic"/>
      </feMerge>
    </filter>
    <filter id="bien-srcalpha" x="-50%" y="-50%" width="200%" height="200%">
      <feGaussianBlur in="SourceAlpha" stdDeviation="4" result="b"/>
      <feOffset in="b" dx="5" dy="6" result="o"/>
      <feMerge>
        <feMergeNode in="o"/>
        <feMergeNode in="SourceGraphic"/>
      </feMerge>
    </filter>
  </defs>

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">sombras: siempre SourceAlpha</text>

  <rect x="15" y="35" width="130" height="155" fill="#fef2f2" stroke="#ef4444" stroke-width="1.5" rx="4"/>
  <text x="80" y="52" text-anchor="middle" font-size="9" fill="#9d174d" font-weight="bold">MAL</text>
  <text x="80" y="66" text-anchor="middle" font-size="7" fill="#64748b">in="SourceGraphic"</text>
  <circle cx="80" cy="110" r="28" fill="#ec4899" filter="url(#mal-srcgraphic)"/>
  <text x="80" y="170" text-anchor="middle" font-size="7" fill="#ef4444">sombra rosada (hereda color)</text>

  <rect x="155" y="35" width="130" height="155" fill="#dcfce7" stroke="#10b981" stroke-width="1.5" rx="4"/>
  <text x="220" y="52" text-anchor="middle" font-size="9" fill="#166534" font-weight="bold">BIEN</text>
  <text x="220" y="66" text-anchor="middle" font-size="7" fill="#64748b">in="SourceAlpha"</text>
  <circle cx="220" cy="110" r="28" fill="#ec4899" filter="url(#bien-srcalpha)"/>
  <text x="220" y="170" text-anchor="middle" font-size="7" fill="#166534">sombra neutra (correcto)</text>
</svg>
```

---

## 3. Olvidar `feMergeNode in="SourceGraphic"` — se pierde el original

Si montas una sombra manualmente y terminas con el blur desplazado, el `<feMerge>` final debe incluir **también** el `SourceGraphic`. Si no, el resultado es solo la sombra: el elemento original desaparece por completo.

```svg
<svg width="300" height="200"
     viewBox="0 0 300 200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <!-- MAL: merge solo con la sombra → el original desaparece -->
    <filter id="mal-no-src" x="-50%" y="-50%" width="200%" height="200%">
      <feGaussianBlur in="SourceAlpha" stdDeviation="3" result="b"/>
      <feOffset in="b" dx="4" dy="5" result="s"/>
      <feMerge>
        <feMergeNode in="s"/>
      </feMerge>
    </filter>
    <!-- BIEN: merge con sombra + SourceGraphic -->
    <filter id="bien-con-src" x="-50%" y="-50%" width="200%" height="200%">
      <feGaussianBlur in="SourceAlpha" stdDeviation="3" result="b"/>
      <feOffset in="b" dx="4" dy="5" result="s"/>
      <feMerge>
        <feMergeNode in="s"/>
        <feMergeNode in="SourceGraphic"/>
      </feMerge>
    </filter>
  </defs>

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">sin SourceGraphic en el merge, se pierde el original</text>

  <rect x="15" y="35" width="130" height="155" fill="#fef2f2" stroke="#ef4444" stroke-width="1.5" rx="4"/>
  <text x="80" y="52" text-anchor="middle" font-size="9" fill="#9d174d" font-weight="bold">MAL</text>
  <text x="80" y="66" text-anchor="middle" font-size="7" fill="#64748b">solo sombra en merge</text>
  <rect x="50" y="85" width="60" height="60" rx="6" fill="#f59e0b" filter="url(#mal-no-src)"/>
  <text x="80" y="170" text-anchor="middle" font-size="7" fill="#ef4444">desapareció el naranja</text>

  <rect x="155" y="35" width="130" height="155" fill="#dcfce7" stroke="#10b981" stroke-width="1.5" rx="4"/>
  <text x="220" y="52" text-anchor="middle" font-size="9" fill="#166534" font-weight="bold">BIEN</text>
  <text x="220" y="66" text-anchor="middle" font-size="7" fill="#64748b">sombra + SourceGraphic</text>
  <rect x="190" y="85" width="60" height="60" rx="6" fill="#f59e0b" filter="url(#bien-con-src)"/>
  <text x="220" y="170" text-anchor="middle" font-size="7" fill="#166534">el rect se ve sobre su sombra</text>
</svg>
```

---

## 4. Referencias `in`/`result` mal encadenadas

Si una primitiva referencia un `result` que nunca se definió, o referencia uno que se define **después**, el comportamiento es indefinido — muchos navegadores simplemente usan `SourceGraphic` por defecto y el efecto se rompe en silencio. Siempre encadenar de arriba hacia abajo.

```svg
<svg width="300" height="210"
     viewBox="0 0 300 210"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <!-- MAL: typo en el result — "blr" en lugar de "blur" -->
    <filter id="mal-typo" x="-50%" y="-50%" width="200%" height="200%">
      <feGaussianBlur in="SourceAlpha" stdDeviation="4" result="blur"/>
      <feOffset in="blr" dx="4" dy="5" result="off"/>  <!-- typo: "blr" -->
      <feMerge>
        <feMergeNode in="off"/>
        <feMergeNode in="SourceGraphic"/>
      </feMerge>
    </filter>
    <!-- BIEN: nombres consistentes -->
    <filter id="bien-nombres" x="-50%" y="-50%" width="200%" height="200%">
      <feGaussianBlur in="SourceAlpha" stdDeviation="4" result="blur"/>
      <feOffset in="blur" dx="4" dy="5" result="off"/>
      <feMerge>
        <feMergeNode in="off"/>
        <feMergeNode in="SourceGraphic"/>
      </feMerge>
    </filter>
  </defs>

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">un typo en "in" rompe la cadena</text>

  <rect x="15" y="35" width="130" height="160" fill="#fef2f2" stroke="#ef4444" stroke-width="1.5" rx="4"/>
  <text x="80" y="52" text-anchor="middle" font-size="9" fill="#9d174d" font-weight="bold">MAL</text>
  <text x="80" y="66" text-anchor="middle" font-size="7" fill="#64748b">in="blr" (typo)</text>
  <circle cx="80" cy="115" r="28" fill="#8b5cf6" filter="url(#mal-typo)"/>
  <text x="80" y="168" text-anchor="middle" font-size="7" fill="#ef4444">la sombra no usa el blur</text>
  <text x="80" y="180" text-anchor="middle" font-size="7" fill="#ef4444">o cae a SourceGraphic</text>

  <rect x="155" y="35" width="130" height="160" fill="#dcfce7" stroke="#10b981" stroke-width="1.5" rx="4"/>
  <text x="220" y="52" text-anchor="middle" font-size="9" fill="#166534" font-weight="bold">BIEN</text>
  <text x="220" y="66" text-anchor="middle" font-size="7" fill="#64748b">in="blur" coincide</text>
  <circle cx="220" cy="115" r="28" fill="#8b5cf6" filter="url(#bien-nombres)"/>
  <text x="220" y="168" text-anchor="middle" font-size="7" fill="#166534">encadenamiento correcto</text>
</svg>
```

---

## 5. Aplicar filtros costosos a elementos grandes o animados

Los filtros procesan **cada pixel** del área del filtro. Un `feGaussianBlur` con `stdDeviation="20"` sobre un elemento que cubre 800×600 procesa cientos de miles de pixels por frame. Si además animas `stdDeviation`, el navegador re-ejecuta el pipeline en cada fotograma — el framerate se desploma.

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">coste de los filtros</text>

  <!-- MAL: blur enorme sobre un elemento grande -->
  <rect x="15" y="35" width="270" height="85" fill="#fef2f2" stroke="#ef4444" stroke-width="1.5" rx="4"/>
  <text x="25" y="52" font-size="9" font-weight="bold" fill="#9d174d">MAL · caro</text>
  <text x="25" y="66" font-size="7" fill="#64748b">&lt;rect width="800" height="600"</text>
  <text x="25" y="77" font-size="7" fill="#64748b">      filter="url(#blur-20)"/&gt;</text>
  <text x="25" y="88" font-size="7" fill="#64748b">&lt;filter id="blur-20"&gt;</text>
  <text x="25" y="99" font-size="7" fill="#64748b">  &lt;feGaussianBlur stdDeviation="20"/&gt;</text>
  <text x="25" y="110" font-size="7" fill="#64748b">&lt;/filter&gt;   ← cada pixel procesado</text>

  <!-- BIEN: alternativas -->
  <rect x="15" y="130" width="270" height="100" fill="#dcfce7" stroke="#10b981" stroke-width="1.5" rx="4"/>
  <text x="25" y="147" font-size="9" font-weight="bold" fill="#166534">BIEN · estrategias</text>
  <text x="25" y="161" font-size="7" fill="#64748b">• filtrar elementos pequeños, no el canvas entero</text>
  <text x="25" y="173" font-size="7" fill="#64748b">• limitar el filter region con x/y/width/height</text>
  <text x="25" y="185" font-size="7" fill="#64748b">• nunca animar stdDeviation en elementos grandes</text>
  <text x="25" y="197" font-size="7" fill="#64748b">• animar opacity o transform entre dos versiones pre-renderizadas</text>
  <text x="25" y="209" font-size="7" fill="#64748b">• para blur uniforme: CSS backdrop-filter (acelerado por GPU)</text>
  <text x="25" y="221" font-size="7" fill="#64748b">• agrupar varios efectos en un solo &lt;filter&gt; (un pipeline, no N)</text>
</svg>
```
