# 7.4 Uniones de línea (`stroke-linejoin`)

`stroke-linejoin` controla la forma de la **esquina** donde dos segmentos de trazo se encuentran. Aplica a polilíneas, polígonos y paths con cambios de dirección.

---

## Los tres valores

```svg
<svg width="300" height="160"
     viewBox="0 0 300 160"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- miter (default): punta afilada -->
  <polyline points="20,120 60,30 100,120"
            fill="none" stroke="#3b82f6" stroke-width="14" stroke-linejoin="miter"/>
  <text x="60" y="145" text-anchor="middle" font-size="8" fill="#3b82f6">miter</text>
  <text x="60" y="156" text-anchor="middle" font-size="7" fill="#94a3b8">punta (defecto)</text>

  <!-- round: esquina redondeada -->
  <polyline points="120,120 160,30 200,120"
            fill="none" stroke="#10b981" stroke-width="14" stroke-linejoin="round"/>
  <text x="160" y="145" text-anchor="middle" font-size="8" fill="#10b981">round</text>
  <text x="160" y="156" text-anchor="middle" font-size="7" fill="#94a3b8">redondeada</text>

  <!-- bevel: esquina achaflanada -->
  <polyline points="220,120 260,30 300,120"
            fill="none" stroke="#f59e0b" stroke-width="14" stroke-linejoin="bevel"/>
  <text x="260" y="145" text-anchor="middle" font-size="8" fill="#f59e0b">bevel</text>
  <text x="260" y="156" text-anchor="middle" font-size="7" fill="#94a3b8">cortada</text>
</svg>
```

---

## `stroke-miterlimit` — controla la longitud de la punta `miter`

Para ángulos muy agudos, la punta `miter` puede crecer enormemente. `stroke-miterlimit` la limita.

```svg
<svg width="300" height="150"
     viewBox="0 0 300 150"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Ángulo agudo con miter default (miterlimit=4) → cambia a bevel -->
  <polyline points="30,120 60,20 90,120"
            fill="none" stroke="#3b82f6" stroke-width="10"
            stroke-linejoin="miter" stroke-miterlimit="4"/>
  <text x="60" y="138" text-anchor="middle" font-size="7.5" fill="#3b82f6">miterlimit=4</text>
  <text x="60" y="149" text-anchor="middle" font-size="7" fill="#94a3b8">punta se corta</text>

  <!-- Miterlimit alto: la punta puede crecer -->
  <polyline points="120,120 150,20 180,120"
            fill="none" stroke="#f59e0b" stroke-width="10"
            stroke-linejoin="miter" stroke-miterlimit="20"/>
  <text x="150" y="138" text-anchor="middle" font-size="7.5" fill="#f59e0b">miterlimit=20</text>
  <text x="150" y="149" text-anchor="middle" font-size="7" fill="#94a3b8">punta larga</text>

  <!-- Miterlimit=1: siempre bevel (nunca miter) -->
  <polyline points="210,120 240,20 270,120"
            fill="none" stroke="#ef4444" stroke-width="10"
            stroke-linejoin="miter" stroke-miterlimit="1"/>
  <text x="240" y="138" text-anchor="middle" font-size="7.5" fill="#ef4444">miterlimit=1</text>
  <text x="240" y="149" text-anchor="middle" font-size="7" fill="#94a3b8">= siempre bevel</text>
</svg>
```

---

## Combinando `linecap` y `linejoin`

```svg
<svg width="300" height="130"
     viewBox="0 0 300 130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#0f172a;display:block; font-family:sans-serif;">

  <!-- Línea suave: round + round (aspecto de "pincel") -->
  <polyline points="15,90 55,30 95,70 135,20"
            fill="none" stroke="#3b82f6" stroke-width="10"
            stroke-linecap="round" stroke-linejoin="round"/>
  <text x="75" y="112" text-anchor="middle" font-size="7.5" fill="#3b82f6">round + round</text>
  <text x="75" y="124" text-anchor="middle" font-size="7" fill="#475569">aspecto orgánico</text>

  <!-- Línea técnica: butt + miter (aspecto preciso) -->
  <polyline points="165,90 205,30 245,70 285,20"
            fill="none" stroke="#f59e0b" stroke-width="10"
            stroke-linecap="butt" stroke-linejoin="miter"/>
  <text x="225" y="112" text-anchor="middle" font-size="7.5" fill="#f59e0b">butt + miter</text>
  <text x="225" y="124" text-anchor="middle" font-size="7" fill="#475569">aspecto técnico</text>
</svg>
```

---

## `stroke-linejoin` en formas cerradas

En polígonos y rectángulos, `linejoin` controla la forma de todas las esquinas.

```svg
<svg width="300" height="100"
     viewBox="0 0 300 100"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Rectángulo con miter (default) -->
  <rect x="10" y="20" width="70" height="55" fill="#dbeafe" stroke="#3b82f6" stroke-width="8" stroke-linejoin="miter"/>
  <text x="45" y="88" text-anchor="middle" font-size="7.5" fill="#3b82f6">miter</text>

  <!-- Rectángulo con round -->
  <rect x="115" y="20" width="70" height="55" fill="#dcfce7" stroke="#10b981" stroke-width="8" stroke-linejoin="round"/>
  <text x="150" y="88" text-anchor="middle" font-size="7.5" fill="#10b981">round</text>

  <!-- Rectángulo con bevel -->
  <rect x="220" y="20" width="70" height="55" fill="#fef9c3" stroke="#f59e0b" stroke-width="8" stroke-linejoin="bevel"/>
  <text x="255" y="88" text-anchor="middle" font-size="7.5" fill="#f59e0b">bevel</text>
</svg>
```
