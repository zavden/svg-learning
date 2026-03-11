# 5.3 Comandos de movimiento: `M` / `m`

`M` mueve el "lápiz" sin dibujar nada. Todo atributo `d` debe comenzar con `M` o `m`.

---

## Qué hace `M`

Establece la posición actual sin trazar ninguna línea. Establece también el "punto de inicio" al que regresará `Z`.

```svg
<svg width="300" height="130"
     viewBox="0 0 300 130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Movimiento invisible seguido de una línea -->
  <path d="M 40 100 L 160 30"
        stroke="#3b82f6" stroke-width="2.5" fill="none" stroke-linecap="round"/>

  <!-- Ilustrar que M no dibuja -->
  <circle cx="40"  cy="100" r="5" fill="#ef4444"/>
  <circle cx="160" cy="30"  r="5" fill="#3b82f6"/>

  <text x="40"  y="115" text-anchor="middle" font-size="8" fill="#ef4444">M: posición</text>
  <text x="40"  y="126" text-anchor="middle" font-size="7" fill="#94a3b8">sin dibujar</text>
  <text x="160" y="18"  text-anchor="middle" font-size="8" fill="#3b82f6">L: destino</text>

  <!-- Separador -->
  <line x1="210" y1="0" x2="210" y2="130" stroke="#e2e8f0" stroke-width="1"/>

  <!-- M vs m relativo -->
  <circle cx="250" cy="65" r="4" fill="#94a3b8"/>
  <text x="250" y="50" text-anchor="middle" font-size="7" fill="#64748b">posición actual</text>
  <text x="250" y="58" text-anchor="middle" font-size="7" fill="#64748b">(250, 65)</text>

  <!-- M absoluto: siempre va al mismo punto -->
  <text x="250" y="80" text-anchor="middle" font-size="7.5" fill="#3b82f6">M 270 40</text>
  <text x="250" y="91" text-anchor="middle" font-size="6.5" fill="#94a3b8">→ va a (270, 40)</text>

  <!-- m relativo: depende de la posición actual -->
  <text x="250" y="106" text-anchor="middle" font-size="7.5" fill="#10b981">m 20 -25</text>
  <text x="250" y="117" text-anchor="middle" font-size="6.5" fill="#94a3b8">→ va a (270, 40) desde (250,65)</text>
</svg>
```

---

## El primer comando siempre es `M`

Sin `M` inicial, el path no se renderiza correctamente. Es la única regla obligatoria de la sintaxis.

```svg
<svg width="300" height="100"
     viewBox="0 0 300 100"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- MAL: sin M inicial (comportamiento indefinido) -->
  <rect x="5" y="5" width="135" height="80" rx="4" fill="#fef2f2" stroke="#fecaca" stroke-width="1"/>
  <!-- path sin M: comportamiento dependiente del navegador -->
  <path d="L 60 20 L 120 70" stroke="#ef4444" stroke-width="2" fill="none"/>
  <text x="70" y="96" text-anchor="middle" font-size="8" fill="#ef4444">sin M → indefinido</text>

  <!-- BIEN: con M inicial -->
  <rect x="155" y="5" width="140" height="80" rx="4" fill="#f0fdf4" stroke="#bbf7d0" stroke-width="1"/>
  <path d="M 170 70 L 230 20 L 285 70" stroke="#10b981" stroke-width="2" fill="none" stroke-linejoin="round"/>
  <text x="225" y="96" text-anchor="middle" font-size="8" fill="#10b981">con M → correcto ✓</text>
</svg>
```

---

## Múltiples `M`: sub-trazados desconectados

Cada `M` adicional inicia un nuevo sub-trazado. Los sub-trazados no están conectados entre sí.

```svg
<svg width="300" height="120"
     viewBox="0 0 300 120"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Un solo path con 3 sub-trazados -->
  <path d="M 20 60 L 80 20 L 80 100 Z
           M 110 60 L 170 20 L 170 100 Z
           M 200 60 L 260 20 L 260 100 Z"
        fill="#6366f1" opacity="0.75" stroke="#4338ca" stroke-width="1.5"/>

  <text x="50"  y="115" text-anchor="middle" font-size="7.5" fill="#64748b">sub-trazado 1</text>
  <text x="140" y="115" text-anchor="middle" font-size="7.5" fill="#64748b">sub-trazado 2</text>
  <text x="230" y="115" text-anchor="middle" font-size="7.5" fill="#64748b">sub-trazado 3</text>

  <!-- Etiqueta indicando que es un solo elemento -->
  <text x="150" y="10" text-anchor="middle" font-size="8" fill="#4338ca">un solo &lt;path&gt; — tres M</text>
</svg>
```

---

## `M` vs `m`: cuándo usar cada uno

```svg
<svg width="300" height="120"
     viewBox="0 0 300 120"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- M absoluto: más predecible para posicionar en coordenadas específicas -->
  <path d="M 50 60 L 110 20 L 110 100 Z" fill="#3b82f6" opacity="0.8"/>
  <text x="80" y="112" text-anchor="middle" font-size="7.5" fill="#3b82f6">M absoluto</text>
  <text x="80" y="10" text-anchor="middle" font-size="7" fill="#94a3b8">coordenadas fijas</text>

  <!-- m relativo: permite mover todo el path cambiando solo el M inicial -->
  <!-- Si cambias M 150 a M 200, el path se desplaza 50px sin cambiar nada más -->
  <path d="M 190 60 l 60 -40 l 0 80 Z" fill="#10b981" opacity="0.8"/>
  <text x="220" y="112" text-anchor="middle" font-size="7.5" fill="#10b981">m relativo</text>
  <text x="220" y="10" text-anchor="middle" font-size="7" fill="#94a3b8">portable — cambia M = todo se mueve</text>
</svg>
```

**Regla práctica:** usa `M` absoluto para posicionar en un lugar específico. Usa `m` relativo (y minúsculas en general) cuando quieras que el path sea portable — cambiar solo el primer `M` mueve todo el dibujo.
