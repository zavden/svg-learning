# Cheat sheet — Recorte y Máscaras

---

## clipPath vs mask en una mirada

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" fill="#f8fafc" font-weight="bold">
    clipPath vs mask
  </text>

  <!-- clipPath -->
  <rect x="10" y="34" width="135" height="175" fill="#dbeafe" stroke="#3b82f6" stroke-width="1.5" rx="4"/>
  <text x="77" y="52" text-anchor="middle" font-size="10" font-weight="bold" fill="#1e40af">&lt;clipPath&gt;</text>

  <text x="20" y="72"  font-size="7" fill="#1e293b" font-weight="bold">Binario</text>
  <text x="20" y="84"  font-size="7" fill="#64748b">dentro = visible</text>
  <text x="20" y="94"  font-size="7" fill="#64748b">fuera = oculto</text>

  <text x="20" y="112" font-size="7" fill="#1e293b" font-weight="bold">Basado en</text>
  <text x="20" y="124" font-size="7" fill="#64748b">geometría pura</text>
  <text x="20" y="134" font-size="7" fill="#64748b">ignora fill/stroke</text>

  <text x="20" y="152" font-size="7" fill="#1e293b" font-weight="bold">Rendimiento</text>
  <text x="20" y="164" font-size="7" fill="#10b981">ligero, rápido</text>

  <text x="20" y="182" font-size="7" fill="#1e293b" font-weight="bold">Uso típico</text>
  <text x="20" y="194" font-size="7" fill="#64748b">recortar imagen en</text>
  <text x="20" y="204" font-size="7" fill="#64748b">círculo, estrella, texto</text>

  <!-- mask -->
  <rect x="155" y="34" width="135" height="175" fill="#dcfce7" stroke="#10b981" stroke-width="1.5" rx="4"/>
  <text x="222" y="52" text-anchor="middle" font-size="10" font-weight="bold" fill="#166534">&lt;mask&gt;</text>

  <text x="165" y="72"  font-size="7" fill="#1e293b" font-weight="bold">Gradual</text>
  <text x="165" y="84"  font-size="7" fill="#64748b">blanco = visible</text>
  <text x="165" y="94"  font-size="7" fill="#64748b">negro = oculto</text>
  <text x="165" y="104" font-size="7" fill="#64748b">gris = parcial</text>

  <text x="165" y="122" font-size="7" fill="#1e293b" font-weight="bold">Basado en</text>
  <text x="165" y="134" font-size="7" fill="#64748b">luminancia</text>
  <text x="165" y="144" font-size="7" fill="#64748b">permite gradientes</text>

  <text x="165" y="162" font-size="7" fill="#1e293b" font-weight="bold">Rendimiento</text>
  <text x="165" y="174" font-size="7" fill="#f59e0b">algo más costoso</text>

  <text x="165" y="192" font-size="7" fill="#1e293b" font-weight="bold">Uso típico</text>
  <text x="165" y="204" font-size="7" fill="#64748b">fade, viñeta, overlay</text>

  <text x="150" y="218" text-anchor="middle" font-size="7" fill="#94a3b8">
    ¿semitransparencia? → mask. Si no, → clipPath
  </text>
</svg>
```

---

## Esqueletos de código: copiar y pegar

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" fill="#f8fafc" font-weight="bold">
    Snippets base
  </text>

  <!-- clipPath básico -->
  <rect x="10" y="34" width="280" height="50" fill="#e0e7ff" stroke="#6366f1" stroke-width="1" rx="3"/>
  <text x="18" y="47" font-size="7" font-weight="bold" fill="#312e81">clipPath — forma simple</text>
  <text x="18" y="60" font-size="6.5" fill="#1e293b" font-family="monospace">&lt;defs&gt;&lt;clipPath id="clip"&gt;&lt;circle cx="50" cy="50" r="40"/&gt;</text>
  <text x="18" y="70" font-size="6.5" fill="#1e293b" font-family="monospace">&lt;/clipPath&gt;&lt;/defs&gt;</text>
  <text x="18" y="80" font-size="6.5" fill="#1e293b" font-family="monospace">&lt;rect ... clip-path="url(#clip)"/&gt;</text>

  <!-- mask con agujero -->
  <rect x="10" y="92" width="280" height="58" fill="#dcfce7" stroke="#10b981" stroke-width="1" rx="3"/>
  <text x="18" y="105" font-size="7" font-weight="bold" fill="#166534">mask — recorte binario equivalente</text>
  <text x="18" y="118" font-size="6.5" fill="#1e293b" font-family="monospace">&lt;mask id="m"&gt;</text>
  <text x="18" y="128" font-size="6.5" fill="#1e293b" font-family="monospace">  &lt;rect width="100%" height="100%" fill="white"/&gt;</text>
  <text x="18" y="138" font-size="6.5" fill="#1e293b" font-family="monospace">  &lt;circle cx="50" cy="50" r="30" fill="black"/&gt;</text>
  <text x="18" y="148" font-size="6.5" fill="#1e293b" font-family="monospace">&lt;/mask&gt;  → "agujero" circular</text>

  <!-- mask con fade -->
  <rect x="10" y="158" width="280" height="58" fill="#fef9c3" stroke="#f59e0b" stroke-width="1" rx="3"/>
  <text x="18" y="171" font-size="7" font-weight="bold" fill="#92400e">mask — fade lineal</text>
  <text x="18" y="184" font-size="6.5" fill="#1e293b" font-family="monospace">&lt;linearGradient id="g"&gt;&lt;stop offset="0" stop-color="white"/&gt;</text>
  <text x="18" y="194" font-size="6.5" fill="#1e293b" font-family="monospace">  &lt;stop offset="1" stop-color="black"/&gt;&lt;/linearGradient&gt;</text>
  <text x="18" y="204" font-size="6.5" fill="#1e293b" font-family="monospace">&lt;mask id="m"&gt;&lt;rect width="100%" height="100%" fill="url(#g)"/&gt;</text>
  <text x="18" y="214" font-size="6.5" fill="#1e293b" font-family="monospace">&lt;/mask&gt;</text>

  <!-- CSS clip-path -->
  <rect x="10" y="222" width="280" height="14" fill="#fce7f3" stroke="#ec4899" stroke-width="1" rx="3"/>
  <text x="18" y="232" font-size="6.5" fill="#1e293b" font-family="monospace">CSS: .el { clip-path: circle(50%); } ó url(#clip);</text>
</svg>
```

---

## Formas CSS `clip-path` — galería

```svg
<svg width="300" height="200"
     viewBox="0 0 300 200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" fill="#f8fafc" font-weight="bold">
    CSS clip-path — galería visual
  </text>

  <!-- circle() -->
  <rect x="15" y="40" width="50" height="50" fill="#3b82f6"
        style="clip-path: circle(50%);"/>
  <text x="40" y="105" text-anchor="middle" font-size="6.5" fill="#64748b">circle(50%)</text>

  <!-- ellipse() -->
  <rect x="80" y="40" width="50" height="50" fill="#10b981"
        style="clip-path: ellipse(50% 35%);"/>
  <text x="105" y="105" text-anchor="middle" font-size="6.5" fill="#64748b">ellipse(50% 35%)</text>

  <!-- inset() -->
  <rect x="145" y="40" width="50" height="50" fill="#ec4899"
        style="clip-path: inset(10%);"/>
  <text x="170" y="105" text-anchor="middle" font-size="6.5" fill="#64748b">inset(10%)</text>

  <!-- inset() round -->
  <rect x="210" y="40" width="50" height="50" fill="#f59e0b"
        style="clip-path: inset(8% round 30%);"/>
  <text x="235" y="105" text-anchor="middle" font-size="6.5" fill="#64748b">inset round</text>

  <!-- triangle -->
  <rect x="15" y="120" width="50" height="50" fill="#8b5cf6"
        style="clip-path: polygon(50% 0, 100% 100%, 0 100%);"/>
  <text x="40" y="185" text-anchor="middle" font-size="6.5" fill="#64748b">triángulo</text>

  <!-- diamante -->
  <rect x="80" y="120" width="50" height="50" fill="#3b82f6"
        style="clip-path: polygon(50% 0, 100% 50%, 50% 100%, 0 50%);"/>
  <text x="105" y="185" text-anchor="middle" font-size="6.5" fill="#64748b">diamante</text>

  <!-- pentagon -->
  <rect x="145" y="120" width="50" height="50" fill="#10b981"
        style="clip-path: polygon(50% 0, 100% 40%, 80% 100%, 20% 100%, 0 40%);"/>
  <text x="170" y="185" text-anchor="middle" font-size="6.5" fill="#64748b">pentágono</text>

  <!-- chevron -->
  <rect x="210" y="120" width="50" height="50" fill="#ec4899"
        style="clip-path: polygon(0 0, 75% 0, 100% 50%, 75% 100%, 0 100%, 25% 50%);"/>
  <text x="235" y="185" text-anchor="middle" font-size="6.5" fill="#64748b">chevron</text>
</svg>
```

---

## Patrones frecuentes listos para usar

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" fill="#f8fafc" font-weight="bold">
    Patrones comunes
  </text>

  <defs>
    <linearGradient id="ref-foto" x1="0" y1="0" x2="1" y2="1">
      <stop offset="0" stop-color="#f59e0b"/>
      <stop offset="0.5" stop-color="#ec4899"/>
      <stop offset="1" stop-color="#8b5cf6"/>
    </linearGradient>

    <clipPath id="ref-circ"><circle cx="0.5" cy="0.5" r="0.5"/></clipPath>

    <linearGradient id="ref-fade-h" x1="0" y1="0" x2="1" y2="0">
      <stop offset="0" stop-color="white"/>
      <stop offset="0.7" stop-color="white"/>
      <stop offset="1" stop-color="black"/>
    </linearGradient>
    <mask id="ref-mask-h">
      <rect x="0" y="0" width="100%" height="100%" fill="url(#ref-fade-h)"/>
    </mask>

    <radialGradient id="ref-vig" cx="0.5" cy="0.5" r="0.5">
      <stop offset="0.5" stop-color="white"/>
      <stop offset="1" stop-color="black"/>
    </radialGradient>
    <mask id="ref-mask-vig">
      <rect x="0" y="0" width="100%" height="100%" fill="url(#ref-vig)"/>
    </mask>

    <clipPath id="ref-texto">
      <text x="150" y="215" font-size="30" font-weight="900" text-anchor="middle" font-family="sans-serif">HERO</text>
    </clipPath>
  </defs>

  <!-- Avatar circular -->
  <rect x="20" y="40" width="60" height="60" fill="url(#ref-foto)"
        clip-path="url(#ref-circ)" style="clip-path: url(#ref-circ);"/>
  <text x="50" y="115" text-anchor="middle" font-size="7" fill="#64748b">avatar circular</text>

  <!-- Fade horizontal -->
  <rect x="100" y="40" width="80" height="60" fill="url(#ref-foto)" mask="url(#ref-mask-h)"/>
  <text x="140" y="115" text-anchor="middle" font-size="7" fill="#64748b">fade horizontal</text>

  <!-- Viñeta radial -->
  <rect x="200" y="40" width="80" height="60" fill="url(#ref-foto)" mask="url(#ref-mask-vig)"/>
  <text x="240" y="115" text-anchor="middle" font-size="7" fill="#64748b">viñeta radial</text>

  <!-- Recorte con texto -->
  <rect x="30" y="140" width="240" height="90" fill="url(#ref-foto)" clip-path="url(#ref-texto)"/>
</svg>
```
