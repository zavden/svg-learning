# 12.5 Herencia de atributos de presentación

SVG propaga atributos como `fill`, `stroke` y `font-family` desde los padres a los hijos, igual que CSS hace con las propiedades heredables. Entender qué se hereda — y qué no — evita repetir atributos y sorpresas con la cascada.

---

## Cadena de herencia: raíz → grupo → elemento

```svg
<svg width="300" height="170"
     viewBox="0 0 300 170"
     xmlns="http://www.w3.org/2000/svg"
     fill="#3b82f6" stroke="#1e40af" stroke-width="2"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">herencia en cadena</text>

  <!-- Ninguno de estos elementos tiene fill o stroke: heredan del <svg> raíz -->
  <circle cx="50" cy="80" r="22"/>
  <rect x="90" y="58" width="44" height="44" rx="4"/>

  <!-- Este grupo sobrescribe fill y stroke: sus hijos heredan del grupo -->
  <g fill="#dcfce7" stroke="#166534">
    <circle cx="180" cy="80" r="22"/>
    <rect x="220" y="58" width="44" height="44" rx="4"/>
  </g>

  <text x="70"  y="130" text-anchor="middle" font-size="8" fill="#1e40af">heredan de &lt;svg&gt;</text>
  <text x="70"  y="142" text-anchor="middle" font-size="7" fill="#64748b">fill=#3b82f6</text>

  <text x="220" y="130" text-anchor="middle" font-size="8" fill="#166534">heredan del &lt;g&gt;</text>
  <text x="220" y="142" text-anchor="middle" font-size="7" fill="#64748b">fill=#dcfce7</text>

  <text x="150" y="160" text-anchor="middle" font-size="8" fill="#94a3b8">
    cada nivel puede redefinir lo que cambia
  </text>
</svg>
```

---

## Atributos heredables: `fill`, `stroke`, `font-*`, `color`

```svg
<svg width="300" height="190"
     viewBox="0 0 300 190"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">atributos heredables</text>

  <!-- Todos los hijos del grupo comparten paleta y fuente -->
  <g fill="#4c1d95" stroke="#8b5cf6" stroke-width="2" font-family="Georgia, serif" font-size="12" font-style="italic">
    <rect x="25" y="45" width="80" height="30" rx="4" fill="#ede9fe"/>
    <text x="65" y="65" text-anchor="middle">Serif italic</text>

    <circle cx="165" cy="60" r="18" fill="#ede9fe"/>
    <text x="165" y="64" text-anchor="middle">24</text>

    <polygon points="230,45 270,45 250,80" fill="#ede9fe"/>
    <text x="250" y="73" text-anchor="middle">tri</text>
  </g>

  <!-- Tabla resumen -->
  <g font-family="sans-serif" font-size="8">
    <text x="25"  y="110" fill="#1e293b" font-weight="bold">Se heredan:</text>
    <text x="25"  y="125" fill="#64748b">fill, stroke, stroke-width, stroke-dasharray</text>
    <text x="25"  y="138" fill="#64748b">font-family, font-size, font-weight, font-style</text>
    <text x="25"  y="151" fill="#64748b">color, visibility, cursor, text-anchor</text>
    <text x="25"  y="164" fill="#64748b">letter-spacing, word-spacing, writing-mode</text>
    <text x="25"  y="177" fill="#64748b">fill-opacity, stroke-opacity, fill-rule</text>
  </g>
</svg>
```

---

## Atributos NO heredables: geometría y efectos

```svg
<svg width="300" height="190"
     viewBox="0 0 300 190"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">atributos NO heredables</text>

  <!-- Un grupo con opacity: se aplica al grupo como capa, no a cada hijo -->
  <g opacity="0.5" fill="#ec4899">
    <rect x="25" y="45" width="30" height="30"/>
    <circle cx="80" cy="60" r="15"/>
    <polygon points="110,45 140,45 125,75"/>
  </g>
  <text x="80" y="92" text-anchor="middle" font-size="7" fill="#64748b">opacity al grupo</text>
  <text x="80" y="102" text-anchor="middle" font-size="7" fill="#64748b">(no heredable)</text>

  <!-- clipPath aplicado al grupo como unidad -->
  <defs>
    <clipPath id="demo-clip">
      <circle cx="215" cy="60" r="25"/>
    </clipPath>
  </defs>
  <g clip-path="url(#demo-clip)" fill="#f59e0b">
    <rect x="170" y="45" width="30" height="30"/>
    <circle cx="230" cy="55" r="20"/>
    <rect x="230" y="55" width="30" height="30"/>
  </g>
  <text x="220" y="92"  text-anchor="middle" font-size="7" fill="#64748b">clip-path al grupo</text>
  <text x="220" y="102" text-anchor="middle" font-size="7" fill="#64748b">(no heredable)</text>

  <!-- Tabla -->
  <g font-family="sans-serif" font-size="8">
    <text x="25"  y="125" fill="#1e293b" font-weight="bold">NO se heredan:</text>
    <text x="25"  y="140" fill="#64748b">x, y, width, height (geometría propia)</text>
    <text x="25"  y="153" fill="#64748b">cx, cy, r, rx, ry, x1, y1, x2, y2</text>
    <text x="25"  y="166" fill="#64748b">opacity, clip-path, mask, filter</text>
    <text x="25"  y="179" fill="#64748b">display, overflow, transform</text>
  </g>
</svg>
```

---

## La cascada: atributo vs CSS vs `style` inline

```svg
<svg width="300" height="190"
     viewBox="0 0 300 190"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <style>
    .caja { fill: #10b981; }
  </style>

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">cascada: atributo &lt; CSS &lt; style</text>

  <!-- Caso 1: solo atributo de presentación -->
  <rect x="25" y="40" width="60" height="50" fill="#3b82f6"/>
  <text x="55" y="105" text-anchor="middle" font-size="7" fill="#64748b">fill="#3b82f6"</text>
  <text x="55" y="116" text-anchor="middle" font-size="7" fill="#3b82f6">gana: azul</text>

  <!-- Caso 2: atributo + CSS por clase -->
  <rect x="105" y="40" width="60" height="50" fill="#3b82f6" class="caja"/>
  <text x="135" y="105" text-anchor="middle" font-size="7" fill="#64748b">attr + .caja{fill:verde}</text>
  <text x="135" y="116" text-anchor="middle" font-size="7" fill="#10b981">gana: CSS verde</text>

  <!-- Caso 3: atributo + CSS por clase + style inline -->
  <rect x="185" y="40" width="60" height="50" fill="#3b82f6" class="caja" style="fill:#ec4899"/>
  <text x="215" y="105" text-anchor="middle" font-size="7" fill="#64748b">+ style="fill:rosa"</text>
  <text x="215" y="116" text-anchor="middle" font-size="7" fill="#ec4899">gana: style rosa</text>

  <!-- Resumen textual -->
  <g font-family="sans-serif" font-size="8">
    <text x="25"  y="145" fill="#1e293b" font-weight="bold">Prioridad (de menor a mayor):</text>
    <text x="25"  y="160" fill="#64748b">1. Herencia del padre</text>
    <text x="25"  y="172" fill="#64748b">2. Atributo de presentación (fill="…")</text>
    <text x="150" y="160" fill="#64748b">3. Regla CSS</text>
    <text x="150" y="172" fill="#64748b">4. style inline / !important</text>
  </g>
</svg>
```

---

## Aprovechar la herencia: paleta en el `<svg>` raíz

```svg
<svg width="300" height="160"
     viewBox="0 0 300 160"
     xmlns="http://www.w3.org/2000/svg"
     fill="none" stroke="#1e40af" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">paleta en el &lt;svg&gt; raíz</text>

  <!-- Todos estos iconos heredan fill="none" stroke="#1e40af" del svg -->
  <!-- No hay que repetir el estilo en cada uno -->

  <!-- Icono check -->
  <g transform="translate(50, 75)">
    <circle cx="0" cy="0" r="18"/>
    <path d="M -8 0 L -2 6 L 10 -6"/>
  </g>
  <text x="50" y="125" text-anchor="middle" font-size="8" fill="#1e40af">check</text>

  <!-- Icono flecha -->
  <g transform="translate(120, 75)">
    <path d="M -12 0 L 12 0"/>
    <path d="M 6 -6 L 12 0 L 6 6"/>
  </g>
  <text x="120" y="125" text-anchor="middle" font-size="8" fill="#1e40af">arrow</text>

  <!-- Icono estrella -->
  <g transform="translate(190, 75)">
    <polygon points="0,-14 4,-5 14,-4 6,3 8,13 0,8 -8,13 -6,3 -14,-4 -4,-5"/>
  </g>
  <text x="190" y="125" text-anchor="middle" font-size="8" fill="#1e40af">star</text>

  <!-- Icono plus -->
  <g transform="translate(260, 75)">
    <circle cx="0" cy="0" r="18"/>
    <line x1="-8" y1="0" x2="8" y2="0"/>
    <line x1="0" y1="-8" x2="0" y2="8"/>
  </g>
  <text x="260" y="125" text-anchor="middle" font-size="8" fill="#1e40af">plus</text>

  <text x="150" y="148" text-anchor="middle" font-size="8" fill="#94a3b8">
    mismo estilo sin repetir fill/stroke en cada icono
  </text>
</svg>
```
