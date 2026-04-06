# 12.2 El elemento `<use>`

`<use>` crea **instancias visuales** de un elemento definido en otra parte del SVG. Es el mecanismo central de reutilización: se define una vez, se usa muchas.

---

## La idea básica: definir una vez, usar muchas

```svg
<svg width="300" height="130"
     viewBox="0 0 300 130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">define → referencia → instancia</text>

  <!-- Definición: un árbol simple con id -->
  <defs>
    <g id="arbol">
      <rect x="-3" y="0" width="6" height="15" fill="#92400e"/>
      <circle cx="0" cy="-5" r="12" fill="#166534"/>
      <circle cx="-6" cy="-2" r="8" fill="#10b981"/>
      <circle cx="6" cy="-2" r="8" fill="#10b981"/>
    </g>
  </defs>

  <!-- Suelo -->
  <line x1="20" y1="95" x2="280" y2="95" stroke="#92400e" stroke-width="1"/>

  <!-- Múltiples instancias del mismo árbol: todas comparten la definición -->
  <use href="#arbol" x="40"  y="80"/>
  <use href="#arbol" x="90"  y="80"/>
  <use href="#arbol" x="140" y="80"/>
  <use href="#arbol" x="190" y="80"/>
  <use href="#arbol" x="240" y="80"/>

  <text x="150" y="120" text-anchor="middle" font-size="8" fill="#94a3b8">
    5 instancias &lt;use&gt; — 1 sola definición
  </text>
</svg>
```

---

## `x` e `y`: posicionar la instancia

```svg
<svg width="300" height="150"
     viewBox="0 0 300 150"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">x / y desplazan la instancia</text>

  <!-- El original vive en x=0, y=0: una flecha apuntando a la derecha -->
  <defs>
    <g id="flecha" stroke="#3b82f6" stroke-width="2" fill="#dbeafe">
      <path d="M 0 0 L 25 0 L 25 -5 L 40 5 L 25 15 L 25 10 L 0 10 Z"/>
    </g>
  </defs>

  <!-- Marca de origen -->
  <circle cx="0" cy="40" r="3" fill="#ef4444"/>
  <text x="10" y="44" font-size="7" fill="#ef4444">(0,0) del sistema</text>

  <!-- use x=0 y=0 (por defecto): aparece en el origen del SVG -->
  <use href="#flecha" x="0" y="40"/>
  <text x="20" y="65" font-size="7" fill="#64748b">x=0, y=40</text>

  <!-- use x=100 y=80: desplazada 100 y 80 respecto al elemento original -->
  <use href="#flecha" x="100" y="80"/>
  <text x="120" y="105" font-size="7" fill="#64748b">x=100, y=80</text>

  <!-- use x=200 y=120 -->
  <use href="#flecha" x="180" y="115"/>
  <text x="200" y="140" font-size="7" fill="#64748b">x=180, y=115</text>

  <text x="150" y="30" text-anchor="middle" font-size="8" fill="#94a3b8">
    equivale a transform="translate(x, y)" sobre la instancia
  </text>
</svg>
```

---

## `use` con transformaciones

```svg
<svg width="300" height="170"
     viewBox="0 0 300 170"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">cada instancia puede transformarse</text>

  <!-- Una estrella base, siempre apuntando hacia arriba -->
  <defs>
    <polygon id="estrella"
             points="0,-18 5,-6 17,-6 7,2 11,14 0,7 -11,14 -7,2 -17,-6 -5,-6"
             fill="#f59e0b" stroke="#92400e" stroke-width="1"/>
  </defs>

  <!-- Instancia 1: sin transformación -->
  <use href="#estrella" transform="translate(40, 75)"/>
  <text x="40" y="115" text-anchor="middle" font-size="7" fill="#64748b">original</text>

  <!-- Instancia 2: rotada -->
  <use href="#estrella" transform="translate(100, 75) rotate(30)"/>
  <text x="100" y="115" text-anchor="middle" font-size="7" fill="#64748b">rotate(30)</text>

  <!-- Instancia 3: escalada más pequeña -->
  <use href="#estrella" transform="translate(160, 75) scale(0.6)"/>
  <text x="160" y="115" text-anchor="middle" font-size="7" fill="#64748b">scale(0.6)</text>

  <!-- Instancia 4: escalada más grande y rotada -->
  <use href="#estrella" transform="translate(230, 75) scale(1.3) rotate(-20)"/>
  <text x="230" y="115" text-anchor="middle" font-size="7" fill="#64748b">scale + rotate</text>

  <text x="150" y="150" text-anchor="middle" font-size="8" fill="#94a3b8">
    cada &lt;use&gt; es independiente — el original no cambia
  </text>
</svg>
```

---

## `href` vs `xlink:href`

```svg
<svg width="300" height="150"
     viewBox="0 0 300 150"
     xmlns="http://www.w3.org/2000/svg"
     xmlns:xlink="http://www.w3.org/1999/xlink"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">href (SVG2) vs xlink:href (SVG1.1)</text>

  <defs>
    <circle id="punto" r="10" fill="#ec4899" stroke="#9d174d" stroke-width="2"/>
  </defs>

  <!-- SVG 2: simple y limpio, soportado en todos los navegadores modernos -->
  <use href="#punto" x="50" y="70"/>
  <text x="50" y="95" text-anchor="middle" font-size="8" fill="#3b82f6">href="#punto"</text>
  <text x="50" y="108" text-anchor="middle" font-size="7" fill="#64748b">SVG 2</text>

  <!-- SVG 1.1: sintaxis antigua, requiere declarar xmlns:xlink -->
  <use xlink:href="#punto" x="150" y="70"/>
  <text x="150" y="95" text-anchor="middle" font-size="8" fill="#f59e0b">xlink:href="#punto"</text>
  <text x="150" y="108" text-anchor="middle" font-size="7" fill="#64748b">SVG 1.1 (legacy)</text>

  <!-- Máxima compatibilidad: ambos -->
  <use href="#punto" xlink:href="#punto" x="250" y="70"/>
  <text x="250" y="95" text-anchor="middle" font-size="8" fill="#10b981">ambos</text>
  <text x="250" y="108" text-anchor="middle" font-size="7" fill="#64748b">máxima compatibilidad</text>

  <text x="150" y="135" text-anchor="middle" font-size="8" fill="#94a3b8">
    hoy href basta — xlink:href solo para navegadores muy antiguos
  </text>
</svg>
```

---

## Estilizar instancias con `currentColor`

```svg
<svg width="300" height="150"
     viewBox="0 0 300 150"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">currentColor propaga el color al &lt;use&gt;</text>

  <!-- El corazón usa currentColor en lugar de un color fijo -->
  <defs>
    <path id="corazon"
          d="M 0 -5 C -8 -18, -22 -10, 0 10 C 22 -10, 8 -18, 0 -5 Z"
          fill="currentColor"/>
  </defs>

  <!-- Cada <use> tiene su propio color y el corazón lo adopta -->
  <use href="#corazon" x="45" y="75" style="color:#ef4444"/>
  <text x="45" y="115" text-anchor="middle" font-size="7" fill="#64748b">color:#ef4444</text>

  <use href="#corazon" x="110" y="75" style="color:#3b82f6"/>
  <text x="110" y="115" text-anchor="middle" font-size="7" fill="#64748b">color:#3b82f6</text>

  <use href="#corazon" x="175" y="75" style="color:#10b981"/>
  <text x="175" y="115" text-anchor="middle" font-size="7" fill="#64748b">color:#10b981</text>

  <use href="#corazon" x="240" y="75" style="color:#f59e0b"/>
  <text x="240" y="115" text-anchor="middle" font-size="7" fill="#64748b">color:#f59e0b</text>

  <text x="150" y="138" text-anchor="middle" font-size="8" fill="#94a3b8">
    truco clásico: fill="currentColor" + color en cada &lt;use&gt;
  </text>
</svg>
```

---

## Shadow DOM: qué se puede y qué no

```svg
<svg width="300" height="170"
     viewBox="0 0 300 170"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">los hijos del &lt;use&gt; están en un shadow DOM</text>

  <!-- Un icono con múltiples partes -->
  <defs>
    <g id="icono-caja">
      <rect x="-15" y="-12" width="30" height="24" fill="#dbeafe" stroke="#3b82f6" stroke-width="2"/>
      <line x1="-15" y1="-2" x2="15" y2="-2" stroke="#3b82f6" stroke-width="2"/>
      <circle cx="0" cy="5" r="3" fill="#3b82f6"/>
    </g>
  </defs>

  <!-- La instancia se ve bien, pero no se puede estilizar "circle" desde fuera -->
  <use href="#icono-caja" x="60" y="75"/>
  <text x="60"  y="110" text-anchor="middle" font-size="8" fill="#64748b">instancia visible</text>
  <text x="60"  y="122" text-anchor="middle" font-size="7" fill="#64748b">del icono-caja</text>

  <!-- Explicación textual -->
  <g transform="translate(130, 50)">
    <rect x="0" y="0" width="150" height="100" fill="#fef2f2" stroke="#ef4444" stroke-width="1" rx="4"/>
    <text x="75" y="16" text-anchor="middle" font-size="8" font-weight="bold" fill="#ef4444">
      CSS externo NO entra
    </text>
    <text x="10" y="32" font-size="7" fill="#1e293b">use circle { fill: red; }</text>
    <text x="10" y="44" font-size="7" fill="#ef4444">× no aplica</text>

    <text x="10" y="62" font-size="7" fill="#1e293b">use { fill: red; }</text>
    <text x="10" y="74" font-size="7" fill="#10b981">✓ heredado por hijos sin fill</text>

    <text x="10" y="90" font-size="7" fill="#1e293b">currentColor + color:red</text>
    <text x="10" y="100" font-size="7" fill="#10b981" dy="-2">✓ funciona</text>
  </g>
</svg>
```
