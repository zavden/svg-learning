# 13.5 Diferencias entre `<clipPath>` y `<mask>`

`clipPath` y `mask` sirven para lo mismo — ocultar partes de un elemento — pero trabajan de forma distinta. Una usa geometría pura; la otra, luminancia con transparencias.

---

## Recorte binario vs transición gradual

```svg
<svg width="300" height="170"
     viewBox="0 0 300 170"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">clipPath = nítido · mask = suave</text>

  <defs>
    <clipPath id="cp-comp">
      <circle cx="75" cy="85" r="35"/>
    </clipPath>

    <radialGradient id="grad-comp" cx="0.5" cy="0.5" r="0.5">
      <stop offset="0.4" stop-color="white"/>
      <stop offset="1"   stop-color="black"/>
    </radialGradient>
    <mask id="m-comp">
      <rect x="0" y="0" width="300" height="200" fill="black"/>
      <circle cx="225" cy="85" r="45" fill="url(#grad-comp)"/>
    </mask>
  </defs>

  <!-- clipPath: borde nítido -->
  <rect x="25" y="40" width="100" height="90" fill="#3b82f6" clip-path="url(#cp-comp)"/>
  <text x="75" y="145" text-anchor="middle" font-size="7" fill="#1e40af">clipPath</text>
  <text x="75" y="157" text-anchor="middle" font-size="6.5" fill="#64748b">borde nítido</text>

  <!-- mask: borde suave -->
  <rect x="175" y="40" width="100" height="90" fill="#3b82f6" mask="url(#m-comp)"/>
  <text x="225" y="145" text-anchor="middle" font-size="7" fill="#166534">mask</text>
  <text x="225" y="157" text-anchor="middle" font-size="6.5" fill="#64748b">borde suave (con gradiente)</text>
</svg>
```

---

## Lo que clipPath NO puede hacer

```svg
<svg width="300" height="180"
     viewBox="0 0 300 180"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">casos donde solo mask sirve</text>

  <defs>
    <linearGradient id="fondo-g" x1="0" y1="0" x2="1" y2="0">
      <stop offset="0"   stop-color="#f59e0b"/>
      <stop offset="0.5" stop-color="#ec4899"/>
      <stop offset="1"   stop-color="#8b5cf6"/>
    </linearGradient>

    <linearGradient id="m-grad-hv" x1="0" y1="0" x2="0" y2="1">
      <stop offset="0" stop-color="white"/>
      <stop offset="1" stop-color="black"/>
    </linearGradient>
    <mask id="m-fade-v">
      <rect x="0" y="0" width="300" height="200" fill="url(#m-grad-hv)"/>
    </mask>

    <radialGradient id="spot-g" cx="0.5" cy="0.5" r="0.5">
      <stop offset="0"   stop-color="white"/>
      <stop offset="0.5" stop-color="white" stop-opacity="0.7"/>
      <stop offset="1"   stop-color="black"/>
    </radialGradient>
    <mask id="m-spot">
      <rect x="0" y="0" width="300" height="200" fill="black"/>
      <ellipse cx="50%" cy="50%" rx="40%" ry="35%" fill="url(#spot-g)"/>
    </mask>
  </defs>

  <!-- Fade vertical: imposible con clipPath -->
  <rect x="20" y="35" width="80" height="100" fill="url(#fondo-g)" mask="url(#m-fade-v)"/>
  <text x="60" y="150" text-anchor="middle" font-size="7" fill="#64748b">fade vertical</text>
  <text x="60" y="162" text-anchor="middle" font-size="7" fill="#ef4444">clipPath no puede</text>

  <!-- Spotlight: imposible con clipPath -->
  <rect x="115" y="35" width="80" height="100" fill="url(#fondo-g)" mask="url(#m-spot)"/>
  <text x="155" y="150" text-anchor="middle" font-size="7" fill="#64748b">spotlight</text>
  <text x="155" y="162" text-anchor="middle" font-size="7" fill="#ef4444">clipPath no puede</text>

  <!-- Combinación de ambos -->
  <rect x="210" y="35" width="80" height="100" fill="url(#fondo-g)" mask="url(#m-spot)"/>
  <rect x="210" y="35" width="80" height="100" fill="url(#fondo-g)" mask="url(#m-fade-v)" opacity="0.4"/>
  <text x="250" y="150" text-anchor="middle" font-size="7" fill="#64748b">doble máscara</text>
  <text x="250" y="162" text-anchor="middle" font-size="7" fill="#ef4444">clipPath no puede</text>
</svg>
```

---

## Tabla comparativa completa

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" fill="#f8fafc" font-weight="bold">
    clipPath vs mask
  </text>

  <!-- Encabezado -->
  <rect x="10" y="34" width="280" height="18" fill="#e2e8f0"/>
  <line x1="130" y1="34" x2="130" y2="52" stroke="#94a3b8"/>
  <line x1="210" y1="34" x2="210" y2="52" stroke="#94a3b8"/>
  <text x="70"  y="47" text-anchor="middle" font-size="8" font-weight="bold" fill="#1e293b">Aspecto</text>
  <text x="170" y="47" text-anchor="middle" font-size="8" font-weight="bold" fill="#1e40af">clipPath</text>
  <text x="250" y="47" text-anchor="middle" font-size="8" font-weight="bold" fill="#166534">mask</text>

  <!-- Filas -->
  <g font-size="7" fill="#1e293b">
    <line x1="10" y1="70" x2="290" y2="70" stroke="#e2e8f0"/>
    <line x1="130" y1="52" x2="130" y2="220" stroke="#e2e8f0"/>
    <line x1="210" y1="52" x2="210" y2="220" stroke="#e2e8f0"/>

    <text x="15" y="65">Tipo de recorte</text>
    <text x="170" y="65" text-anchor="middle">binario</text>
    <text x="250" y="65" text-anchor="middle">gradual</text>

    <line x1="10" y1="86" x2="290" y2="86" stroke="#e2e8f0"/>
    <text x="15" y="81">Base</text>
    <text x="170" y="81" text-anchor="middle">geometría</text>
    <text x="250" y="81" text-anchor="middle">luminancia</text>

    <line x1="10" y1="102" x2="290" y2="102" stroke="#e2e8f0"/>
    <text x="15" y="97">Gradientes</text>
    <text x="170" y="97" text-anchor="middle" fill="#ef4444">no</text>
    <text x="250" y="97" text-anchor="middle" fill="#10b981">sí</text>

    <line x1="10" y1="118" x2="290" y2="118" stroke="#e2e8f0"/>
    <text x="15" y="113">Semitransparencia</text>
    <text x="170" y="113" text-anchor="middle" fill="#ef4444">no</text>
    <text x="250" y="113" text-anchor="middle" fill="#10b981">sí</text>

    <line x1="10" y1="134" x2="290" y2="134" stroke="#e2e8f0"/>
    <text x="15" y="129">Rendimiento</text>
    <text x="170" y="129" text-anchor="middle" fill="#10b981">mejor</text>
    <text x="250" y="129" text-anchor="middle" fill="#f59e0b">algo peor</text>

    <line x1="10" y1="150" x2="290" y2="150" stroke="#e2e8f0"/>
    <text x="15" y="145">Bordes</text>
    <text x="170" y="145" text-anchor="middle">nítidos</text>
    <text x="250" y="145" text-anchor="middle">suaves</text>

    <line x1="10" y1="166" x2="290" y2="166" stroke="#e2e8f0"/>
    <text x="15" y="161">Fondo implícito</text>
    <text x="170" y="161" text-anchor="middle">vacío</text>
    <text x="250" y="161" text-anchor="middle">negro</text>

    <line x1="10" y1="182" x2="290" y2="182" stroke="#e2e8f0"/>
    <text x="15" y="177">fill/stroke del contenido</text>
    <text x="170" y="177" text-anchor="middle">se ignora</text>
    <text x="250" y="177" text-anchor="middle">afecta</text>

    <line x1="10" y1="198" x2="290" y2="198" stroke="#e2e8f0"/>
    <text x="15" y="193">Soporte CSS clip-path</text>
    <text x="170" y="193" text-anchor="middle" fill="#10b981">amplio</text>
    <text x="250" y="193" text-anchor="middle" fill="#f59e0b">parcial (-webkit-)</text>

    <text x="15" y="213">Complejidad</text>
    <text x="170" y="213" text-anchor="middle">menor</text>
    <text x="250" y="213" text-anchor="middle">mayor</text>
  </g>

  <text x="150" y="232" text-anchor="middle" font-size="7" fill="#94a3b8">
    regla general: clipPath cuando basta, mask cuando no
  </text>
</svg>
```

---

## Mismo efecto de dos formas: cuándo elegir cada uno

```svg
<svg width="300" height="170"
     viewBox="0 0 300 170"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">recorte circular: los dos sirven</text>

  <defs>
    <linearGradient id="fondo-cmp" x1="0" y1="0" x2="1" y2="1">
      <stop offset="0" stop-color="#10b981"/>
      <stop offset="1" stop-color="#3b82f6"/>
    </linearGradient>

    <clipPath id="cp-circulo-sel">
      <circle cx="75" cy="80" r="40"/>
    </clipPath>

    <mask id="m-circulo-sel">
      <rect x="0" y="0" width="300" height="200" fill="black"/>
      <circle cx="225" cy="80" r="40" fill="white"/>
    </mask>
  </defs>

  <rect x="20" y="40" width="110" height="85" fill="url(#fondo-cmp)" clip-path="url(#cp-circulo-sel)"/>
  <text x="75" y="145" text-anchor="middle" font-size="7" fill="#1e40af">clipPath ✓</text>
  <text x="75" y="158" text-anchor="middle" font-size="6.5" fill="#64748b">más simple y rápido</text>

  <rect x="170" y="40" width="110" height="85" fill="url(#fondo-cmp)" mask="url(#m-circulo-sel)"/>
  <text x="225" y="145" text-anchor="middle" font-size="7" fill="#166534">mask ✓</text>
  <text x="225" y="158" text-anchor="middle" font-size="6.5" fill="#64748b">innecesariamente complejo</text>

  <text x="150" y="30" text-anchor="middle" font-size="7" fill="#94a3b8">
    si no hay gradientes, prefiere clipPath
  </text>
</svg>
```

---

## Árbol de decisión

```svg
<svg width="300" height="230"
     viewBox="0 0 300 230"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="10" fill="#f8fafc" font-weight="bold">
    ¿clipPath o mask? — árbol de decisión
  </text>

  <!-- Pregunta 1 -->
  <rect x="60" y="40" width="180" height="26" rx="6" fill="#ede9fe" stroke="#8b5cf6" stroke-width="1.5"/>
  <text x="150" y="57" text-anchor="middle" font-size="8" fill="#4c1d95">¿necesito semitransparencia?</text>

  <!-- Líneas -->
  <line x1="110" y1="66" x2="75"  y2="85" stroke="#94a3b8" stroke-width="1"/>
  <line x1="190" y1="66" x2="225" y2="85" stroke="#94a3b8" stroke-width="1"/>
  <text x="90"  y="80" font-size="7" fill="#64748b">sí</text>
  <text x="205" y="80" font-size="7" fill="#64748b">no</text>

  <rect x="25" y="85" width="100" height="26" rx="6" fill="#dcfce7" stroke="#10b981" stroke-width="1.5"/>
  <text x="75" y="102" text-anchor="middle" font-size="9" font-weight="bold" fill="#166534">usa mask</text>

  <!-- Pregunta 2 en la rama "no" -->
  <rect x="175" y="85" width="105" height="26" rx="6" fill="#ede9fe" stroke="#8b5cf6" stroke-width="1.5"/>
  <text x="228" y="102" text-anchor="middle" font-size="8" fill="#4c1d95">¿borde difuminado?</text>

  <line x1="200" y1="111" x2="180" y2="130" stroke="#94a3b8"/>
  <line x1="260" y1="111" x2="270" y2="130" stroke="#94a3b8"/>
  <text x="185" y="126" font-size="7" fill="#64748b">sí</text>
  <text x="264" y="126" font-size="7" fill="#64748b">no</text>

  <rect x="135" y="130" width="90" height="26" rx="6" fill="#dcfce7" stroke="#10b981" stroke-width="1.5"/>
  <text x="180" y="147" text-anchor="middle" font-size="9" font-weight="bold" fill="#166534">usa mask</text>

  <!-- Pregunta 3 -->
  <rect x="230" y="130" width="60" height="26" rx="6" fill="#ede9fe" stroke="#8b5cf6" stroke-width="1.5"/>
  <text x="260" y="147" text-anchor="middle" font-size="8" fill="#4c1d95">¿forma?</text>

  <rect x="160" y="170" width="130" height="30" rx="6" fill="#dbeafe" stroke="#3b82f6" stroke-width="1.5"/>
  <text x="225" y="183" text-anchor="middle" font-size="9" font-weight="bold" fill="#1e40af">usa clipPath</text>
  <text x="225" y="195" text-anchor="middle" font-size="6.5" fill="#64748b">geometría simple y rápida</text>
  <line x1="260" y1="156" x2="225" y2="170" stroke="#94a3b8"/>

  <text x="150" y="220" text-anchor="middle" font-size="7" fill="#94a3b8">
    en la duda: empieza con clipPath, pasa a mask si hace falta gradación
  </text>
</svg>
```
