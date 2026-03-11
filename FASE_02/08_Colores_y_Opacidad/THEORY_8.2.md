# 8.2 Opacidad

SVG tiene tres propiedades distintas para controlar la transparencia. Confundirlas produce efectos inesperados, especialmente con elementos solapados.

---

## Los tres niveles de opacidad

```svg
<svg width="300" height="110"
     viewBox="0 0 300 110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">Los tres niveles de opacidad</text>

  <!-- opacity — todo el elemento -->
  <rect x="15" y="32" width="70" height="55" fill="#3b82f6" stroke="#1e40af" stroke-width="4" opacity="0.5"/>
  <text x="50" y="98" text-anchor="middle" font-size="7" fill="#64748b">opacity="0.5"</text>
  <text x="50" y="107" text-anchor="middle" font-size="6.5" fill="#94a3b8">fill + stroke</text>

  <!-- fill-opacity — solo el relleno -->
  <rect x="115" y="32" width="70" height="55" fill="#3b82f6" stroke="#1e40af" stroke-width="4" fill-opacity="0.5"/>
  <text x="150" y="98" text-anchor="middle" font-size="7" fill="#64748b">fill-opacity="0.5"</text>
  <text x="150" y="107" text-anchor="middle" font-size="6.5" fill="#94a3b8">solo fill</text>

  <!-- stroke-opacity — solo el trazo -->
  <rect x="215" y="32" width="70" height="55" fill="#3b82f6" stroke="#1e40af" stroke-width="4" stroke-opacity="0.3"/>
  <text x="250" y="98" text-anchor="middle" font-size="7" fill="#64748b">stroke-opacity="0.3"</text>
  <text x="250" y="107" text-anchor="middle" font-size="6.5" fill="#94a3b8">solo stroke</text>
</svg>
```

---

## La diferencia crítica: `opacity` en grupos

Con `opacity` en un grupo `<g>`, el grupo se renderiza completamente y luego se aplica la transparencia al resultado compuesto. Los solapamientos internos no son "dobles".

```svg
<svg width="300" height="130"
     viewBox="0 0 300 130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">opacity en grupo vs fill-opacity individual</text>

  <!-- Grupo con opacity: solapamiento no doble -->
  <g opacity="0.5">
    <circle cx="55" cy="70" r="30" fill="#3b82f6"/>
    <circle cx="85" cy="70" r="30" fill="#ef4444"/>
  </g>
  <text x="70" y="115" text-anchor="middle" font-size="7" fill="#3b82f6">&lt;g opacity="0.5"&gt;</text>
  <text x="70" y="124" text-anchor="middle" font-size="6.5" fill="#94a3b8">solapamiento NO doble</text>

  <!-- Cada círculo con fill-opacity: solapamiento doble -->
  <circle cx="185" cy="70" r="30" fill="#3b82f6" fill-opacity="0.5"/>
  <circle cx="215" cy="70" r="30" fill="#ef4444" fill-opacity="0.5"/>
  <text x="200" y="115" text-anchor="middle" font-size="7" fill="#3b82f6">fill-opacity="0.5" c/u</text>
  <text x="200" y="124" text-anchor="middle" font-size="6.5" fill="#94a3b8">solapamiento MÁS opaco</text>
</svg>
```

---

## Opacidad multiplicativa en la jerarquía

```svg
<svg width="300" height="110"
     viewBox="0 0 300 110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">opacity se multiplica en jerarquía</text>

  <!-- opacidad plena -->
  <rect x="15" y="35" width="60" height="55" fill="#3b82f6" opacity="1"/>
  <text x="45" y="100" text-anchor="middle" font-size="7" fill="#64748b">1.0</text>

  <!-- padre 0.5 -->
  <g opacity="0.5">
    <rect x="90" y="35" width="60" height="55" fill="#3b82f6"/>
  </g>
  <text x="120" y="100" text-anchor="middle" font-size="7" fill="#64748b">g:0.5 → 0.5</text>

  <!-- padre 0.5, hijo 0.5 → 0.25 -->
  <g opacity="0.5">
    <rect x="165" y="35" width="60" height="55" fill="#3b82f6" opacity="0.5"/>
  </g>
  <text x="195" y="100" text-anchor="middle" font-size="7" fill="#64748b">g:0.5 × elem:0.5</text>
  <text x="195" y="109" text-anchor="middle" font-size="6.5" fill="#94a3b8">= 0.25</text>

  <!-- plena con opacity de número -->
  <rect x="240" y="35" width="60" height="55" fill="#3b82f6" opacity="0.25"/>
  <text x="270" y="100" text-anchor="middle" font-size="7" fill="#64748b">0.25 directo</text>
</svg>
```

---

## Comparativa: `opacity` vs `fill-opacity` vs `stroke-opacity`

```svg
<svg width="300" height="130"
     viewBox="0 0 300 130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">Comparativa de los tres al 50%</text>

  <!-- opacity 0.5: afecta fill Y stroke -->
  <rect x="10" y="30" width="80" height="60" fill="#3b82f6" stroke="#ef4444" stroke-width="6" opacity="0.5"/>
  <text x="50" y="102" text-anchor="middle" font-size="7" fill="#64748b">opacity="0.5"</text>
  <text x="50" y="112" text-anchor="middle" font-size="6.5" fill="#94a3b8">fill + stroke juntos</text>

  <!-- fill-opacity 0.5: solo fill -->
  <rect x="110" y="30" width="80" height="60" fill="#3b82f6" stroke="#ef4444" stroke-width="6" fill-opacity="0.5"/>
  <text x="150" y="102" text-anchor="middle" font-size="7" fill="#64748b">fill-opacity="0.5"</text>
  <text x="150" y="112" text-anchor="middle" font-size="6.5" fill="#94a3b8">stroke opaco</text>

  <!-- stroke-opacity 0.5: solo stroke -->
  <rect x="210" y="30" width="80" height="60" fill="#3b82f6" stroke="#ef4444" stroke-width="6" stroke-opacity="0.3"/>
  <text x="250" y="102" text-anchor="middle" font-size="7" fill="#64748b">stroke-opacity="0.3"</text>
  <text x="250" y="112" text-anchor="middle" font-size="6.5" fill="#94a3b8">fill opaco</text>

  <text x="150" y="127" text-anchor="middle" font-size="7" fill="#94a3b8">todos con fondo blanco para ver el efecto</text>
</svg>
```

---

## Caso de uso: overlay semitransparente

```svg
<svg width="300" height="110"
     viewBox="0 0 300 110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Fondo simulado -->
  <rect x="0" y="0" width="300" height="110" fill="url(#checker)"/>
  <defs>
    <pattern id="checker" x="0" y="0" width="20" height="20" patternUnits="userSpaceOnUse">
      <rect width="10" height="10" fill="#e2e8f0"/>
      <rect x="10" y="10" width="10" height="10" fill="#e2e8f0"/>
    </pattern>
  </defs>

  <!-- Imagen simulada -->
  <rect x="10" y="10" width="130" height="90" fill="#7dd3fc" rx="4"/>
  <circle cx="75" cy="55" r="25" fill="#0284c7"/>

  <!-- Overlay con fill-opacity — el borde sigue opaco -->
  <rect x="10" y="10" width="130" height="90" rx="4"
        fill="#0f172a" fill-opacity="0.5"
        stroke="#334155" stroke-width="2"/>
  <text x="75" y="107" text-anchor="middle" font-size="7" fill="#475569">fill-opacity overlay</text>

  <!-- Overlay con opacity — todo semitransparente -->
  <rect x="160" y="10" width="130" height="90" fill="#7dd3fc" rx="4"/>
  <circle cx="225" cy="55" r="25" fill="#0284c7"/>
  <rect x="160" y="10" width="130" height="90" rx="4"
        fill="#0f172a" stroke="#334155" stroke-width="2" opacity="0.5"/>
  <text x="225" y="107" text-anchor="middle" font-size="7" fill="#475569">opacity overlay</text>
</svg>
```
