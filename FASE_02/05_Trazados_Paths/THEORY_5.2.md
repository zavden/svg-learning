# 5.2 Sintaxis del atributo `d`

El atributo `d` contiene una cadena de comandos y números. Entender sus reglas de sintaxis permite leer y escribir paths a mano.

---

## Absoluto vs relativo: MAYÚSCULA vs minúscula

La misma letra en mayúscula usa coordenadas desde el **origen (0,0)**. En minúscula, desde la **posición actual**.

```svg
<svg width="300" height="140"
     viewBox="0 0 300 140"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Absoluto: L 200 80 siempre va al punto (200,80) -->
  <path d="M 20 80 L 130 30 L 200 80"
        stroke="#3b82f6" stroke-width="2.5" fill="none" stroke-linejoin="round"/>
  <!-- Marcas -->
  <circle cx="20"  cy="80" r="4" fill="#3b82f6"/>
  <circle cx="130" cy="30" r="4" fill="#3b82f6"/>
  <circle cx="200" cy="80" r="4" fill="#3b82f6"/>
  <text x="115" y="12" text-anchor="middle" font-size="8" fill="#3b82f6">L absoluto</text>
  <text x="115" y="22" text-anchor="middle" font-size="7" fill="#94a3b8">M 20 80  L 130 30  L 200 80</text>

  <!-- Coordenadas del origen para referencia -->
  <line x1="20" y1="80" x2="130" y2="80" stroke="#dbeafe" stroke-width="1" stroke-dasharray="2,2"/>
  <text x="75" y="94" text-anchor="middle" font-size="6.5" fill="#93c5fd">va a (130,30)</text>

  <!-- Separador -->
  <line x1="150" y1="0" x2="150" y2="140" stroke="#e2e8f0" stroke-width="1"/>

  <!-- Relativo: l 110 -50 se desplaza 110 a la derecha y 50 arriba -->
  <path d="M 170 80 l 110 -50 l 0 50"
        stroke="#10b981" stroke-width="2.5" fill="none" stroke-linejoin="round"/>
  <circle cx="170" cy="80" r="4" fill="#10b981"/>
  <circle cx="280" cy="30" r="4" fill="#10b981"/>
  <circle cx="280" cy="80" r="4" fill="#10b981"/>
  <text x="225" y="12" text-anchor="middle" font-size="8" fill="#10b981">l relativo</text>
  <text x="225" y="22" text-anchor="middle" font-size="7" fill="#94a3b8">M 170 80  l 110 -50  l 0 50</text>

  <!-- Flecha de desplazamiento -->
  <line x1="170" y1="80" x2="280" y2="80" stroke="#bbf7d0" stroke-width="1" stroke-dasharray="2,2"/>
  <text x="225" y="94" text-anchor="middle" font-size="6.5" fill="#86efac">+110, -50 desde aquí</text>
</svg>
```

---

## Separadores: espacios y comas son equivalentes

Los tres formatos son idénticos para el navegador.

```svg
<svg width="300" height="90"
     viewBox="0 0 300 90"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Los tres producen el mismo triángulo -->
  <path d="M 20 70 L 50 20 L 80 70 Z" fill="#3b82f6" opacity="0.8"/>
  <path d="M110,70 L140,20 L170,70 Z"  fill="#10b981" opacity="0.8"/>
  <path d="M200 70L230 20L260 70Z"     fill="#f59e0b" opacity="0.8"/>

  <text x="50"  y="85" text-anchor="middle" font-size="7" fill="#64748b" font-family="monospace">espacios</text>
  <text x="140" y="85" text-anchor="middle" font-size="7" fill="#64748b" font-family="monospace">comas</text>
  <text x="230" y="85" text-anchor="middle" font-size="7" fill="#64748b" font-family="monospace">sin sep.</text>
</svg>
```

Los números negativos actúan como separadores implícitos: `l10-5` se lee como `l 10 -5`. Los puntos decimales también: `l10.5.3` se lee como `l 10.5 0.3`.

---

## Comandos repetidos: parámetros implícitos

Si hay más números de los necesarios para un comando, se repite ese comando.

```svg
<svg width="300" height="100"
     viewBox="0 0 300 100"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- L con múltiples puntos: una sola L con 6 números = 3 líneas -->
  <path d="M 20 80 L 60 20 100 65 140 20 180 70"
        stroke="#3b82f6" stroke-width="2.5" fill="none" stroke-linejoin="round"/>

  <!-- Puntos marcados -->
  <circle cx="20"  cy="80" r="3" fill="#ef4444"/>
  <circle cx="60"  cy="20" r="3" fill="#3b82f6"/>
  <circle cx="100" cy="65" r="3" fill="#3b82f6"/>
  <circle cx="140" cy="20" r="3" fill="#3b82f6"/>
  <circle cx="180" cy="70" r="3" fill="#3b82f6"/>

  <text x="100" y="90" text-anchor="middle" font-size="7.5" fill="#64748b">
    M 20 80  L 60 20  100 65  140 20  180 70
  </text>
  <text x="100" y="100" text-anchor="middle" font-size="7" fill="#94a3b8">
    = M + 4 comandos L implícitos
  </text>

  <!-- Excepción: M con puntos extra → L implícito -->
  <path d="M 210 80 230 40 270 80"
        stroke="#f59e0b" stroke-width="2.5" fill="none" stroke-linejoin="round"/>
  <circle cx="210" cy="80" r="3" fill="#ef4444"/>
  <circle cx="230" cy="40" r="3" fill="#f59e0b"/>
  <circle cx="270" cy="80" r="3" fill="#f59e0b"/>
  <text x="240" y="100" text-anchor="middle" font-size="7" fill="#94a3b8">M → extra = L</text>
</svg>
```

**Excepción:** después de `M`, los puntos extra se tratan como `L`, no como `M`.

---

## Legibilidad: saltos de línea en `d`

Los saltos de línea dentro del atributo `d` son válidos. Ayudan a leer paths complejos escritos a mano.

```svg-only
<svg width="200" height="80"
     viewBox="0 0 200 80"
     xmlns="http://www.w3.org/2000/svg">

  <!-- Path escrito con saltos de línea para legibilidad -->
  <path d="M 20 60
           L 60 10
           L 100 50
           L 140 10
           L 180 60
           Z"
        fill="#6366f1" opacity="0.8"/>
</svg>
```

```
<!-- Mismo path, en una línea (como lo genera un editor gráfico) -->
<path d="M 20 60 L 60 10 L 100 50 L 140 10 L 180 60 Z"/>
```
