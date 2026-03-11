# 8.3 `currentColor`

`currentColor` es una palabra clave que hereda el valor de la propiedad CSS `color` del contexto. Convierte iconos y gráficos en elementos que se adaptan automáticamente al color del texto circundante.

---

## El concepto: heredar el color del texto

```svg
<svg width="300" height="120"
     viewBox="0 0 300 120"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">currentColor — hereda la prop. CSS "color"</text>

  <!-- Contexto rojo -->
  <g style="color: #ef4444;">
    <circle cx="50" cy="65" r="28" fill="currentColor"/>
    <text x="50" y="104" text-anchor="middle" font-size="7" fill="#ef4444">color: #ef4444</text>
    <text x="50" y="113" text-anchor="middle" font-size="6.5" fill="#94a3b8">fill=currentColor</text>
  </g>

  <!-- Contexto azul -->
  <g style="color: #3b82f6;">
    <circle cx="150" cy="65" r="28" fill="currentColor"/>
    <text x="150" y="104" text-anchor="middle" font-size="7" fill="#3b82f6">color: #3b82f6</text>
    <text x="150" y="113" text-anchor="middle" font-size="6.5" fill="#94a3b8">fill=currentColor</text>
  </g>

  <!-- Contexto verde -->
  <g style="color: #10b981;">
    <circle cx="250" cy="65" r="28" fill="currentColor"/>
    <text x="250" y="104" text-anchor="middle" font-size="7" fill="#10b981">color: #10b981</text>
    <text x="250" y="113" text-anchor="middle" font-size="6.5" fill="#94a3b8">fill=currentColor</text>
  </g>
</svg>
```

---

## Uso principal: iconos adaptativos en botones

El patrón más habitual: el icono SVG usa `fill="currentColor"` y el botón/enlace define el `color` CSS. El icono adopta el color del texto automáticamente.

```svg
<svg width="300" height="130"
     viewBox="0 0 300 130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">Icono adaptativo con currentColor</text>

  <!-- Botón primario -->
  <g style="color: white;">
    <rect x="20" y="32" width="110" height="36" rx="6" fill="#3b82f6"/>
    <!-- Icono estrella con currentColor -->
    <path d="M 42 54 L 45 45 L 48 54 L 57 54 L 50 59 L 53 68 L 45 63 L 37 68 L 40 59 L 33 54 Z"
          fill="currentColor" transform="scale(0.6) translate(30,30)"/>
    <text x="75" y="54" text-anchor="middle" font-size="8.5" fill="currentColor">Favorito</text>
  </g>

  <!-- Botón peligro -->
  <g style="color: white;">
    <rect x="20" y="80" width="110" height="36" rx="6" fill="#ef4444"/>
    <text x="75" y="99" text-anchor="middle" font-size="8.5" fill="currentColor">✕  Eliminar</text>
  </g>

  <!-- Enlace de texto -->
  <g style="color: #3b82f6;">
    <text x="200" y="55" text-anchor="middle" font-size="9" fill="currentColor" text-decoration="underline">Ver más</text>
    <text x="200" y="72" text-anchor="middle" font-size="7" fill="#94a3b8">fill=currentColor</text>
    <text x="200" y="82" text-anchor="middle" font-size="7" fill="#94a3b8">hereda color: #3b82f6</text>
  </g>

  <text x="150" y="120" text-anchor="middle" font-size="7.5" fill="#64748b">
    El icono no necesita saber su color — lo hereda del botón
  </text>
</svg>
```

---

## currentColor en fill y stroke simultáneamente

```svg
<svg width="300" height="100"
     viewBox="0 0 300 100"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Icono con fill semi-opaco y stroke sólido, ambos currentColor -->
  <g style="color: #8b5cf6;">
    <rect x="20" y="18" width="60" height="60" rx="8"
          fill="currentColor" fill-opacity="0.15"
          stroke="currentColor" stroke-width="2"/>
    <text x="50" y="53" text-anchor="middle" font-size="20" fill="currentColor">★</text>
    <text x="50" y="90" text-anchor="middle" font-size="7" fill="#8b5cf6">fill + stroke = currentColor</text>
  </g>

  <g style="color: #f59e0b;">
    <rect x="120" y="18" width="60" height="60" rx="8"
          fill="currentColor" fill-opacity="0.15"
          stroke="currentColor" stroke-width="2"/>
    <text x="150" y="53" text-anchor="middle" font-size="20" fill="currentColor">★</text>
    <text x="150" y="90" text-anchor="middle" font-size="7" fill="#f59e0b">color: #f59e0b</text>
  </g>

  <g style="color: #10b981;">
    <rect x="220" y="18" width="60" height="60" rx="8"
          fill="currentColor" fill-opacity="0.15"
          stroke="currentColor" stroke-width="2"/>
    <text x="250" y="53" text-anchor="middle" font-size="20" fill="currentColor">★</text>
    <text x="250" y="90" text-anchor="middle" font-size="7" fill="#10b981">color: #10b981</text>
  </g>
</svg>
```

---

## Variables CSS para iconos multicolor

Cuando el icono tiene varios colores, `currentColor` no es suficiente. La solución: variables CSS.

```svg
<svg width="300" height="110"
     viewBox="0 0 300 110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">Variables CSS — iconos bicolor</text>

  <!-- Icono bicolor con variables CSS -->
  <g style="--icon-primary: #3b82f6; --icon-secondary: #fbbf24;">
    <circle cx="75" cy="60" r="30" fill="var(--icon-primary)"/>
    <circle cx="75" cy="60" r="15" fill="var(--icon-secondary)"/>
    <text x="75" y="100" text-anchor="middle" font-size="7" fill="#64748b">
      --primary + --secondary
    </text>
  </g>

  <!-- Mismas variables, distintos valores -->
  <g style="--icon-primary: #10b981; --icon-secondary: #fff;">
    <circle cx="225" cy="60" r="30" fill="var(--icon-primary)"/>
    <circle cx="225" cy="60" r="15" fill="var(--icon-secondary)"/>
    <text x="225" y="100" text-anchor="middle" font-size="7" fill="#64748b">
      mismo icono, otro tema
    </text>
  </g>

  <text x="150" y="108" text-anchor="middle" font-size="6.5" fill="#94a3b8">
    var(--icon-color, currentColor) — currentColor como fallback
  </text>
</svg>
```

---

## Limitaciones de `currentColor`

```svg
<svg width="300" height="100"
     viewBox="0 0 300 100"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">Limitaciones</text>

  <text x="10" y="38" font-size="7.5" fill="#ef4444">✗</text>
  <text x="22" y="38" font-size="7" fill="#64748b">Solo representa un color (el de "color")</text>

  <text x="10" y="53" font-size="7.5" fill="#ef4444">✗</text>
  <text x="22" y="53" font-size="7" fill="#64748b">En SVG standalone (.svg), "color" puede no estar definido</text>

  <text x="10" y="68" font-size="7.5" fill="#ef4444">✗</text>
  <text x="22" y="68" font-size="7" fill="#64748b">No funciona para iconos de múltiples colores distintos</text>

  <text x="10" y="83" font-size="7.5" fill="#10b981">✓</text>
  <text x="22" y="83" font-size="7" fill="#64748b">Usar var(--color, currentColor) para añadir fallback seguro</text>

  <text x="10" y="98" font-size="7.5" fill="#10b981">✓</text>
  <text x="22" y="98" font-size="7" fill="#64748b">Variables CSS para iconos con 2+ colores distintos</text>
</svg>
```
