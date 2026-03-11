# 11.3 `scale(sx, sy)`

Escala el sistema de coordenadas desde el origen (0,0). Controla el tamaño visual y puede crear efectos de espejo con valores negativos.

---

## Escala uniforme y no uniforme

```svg
<svg width="300" height="120"
     viewBox="0 0 300 120"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Original (referencia) -->
  <rect x="10" y="30" width="40" height="40" rx="3" fill="#dbeafe" stroke="#3b82f6" stroke-width="1"/>
  <text x="30" y="84" text-anchor="middle" font-size="7" fill="#64748b">original</text>

  <!-- scale uniforme: escala desde el origen (0,0) -->
  <!-- Nota: el elemento también se desplaza porque escala TODO el espacio -->
  <rect x="10" y="30" width="40" height="40" rx="3" fill="#dcfce7" stroke="#10b981" stroke-width="1"
        transform="translate(60,0) scale(1.5)"/>
  <text x="105" y="105" text-anchor="middle" font-size="7" fill="#10b981">scale(1.5)</text>
  <text x="105" y="114" text-anchor="middle" font-size="6.5" fill="#94a3b8">uniforme</text>

  <!-- scale no uniforme: distorsiona -->
  <rect x="10" y="30" width="40" height="40" rx="3" fill="#fef9c3" stroke="#f59e0b" stroke-width="1"
        transform="translate(150,0) scale(2, 0.6)"/>
  <text x="210" y="100" text-anchor="middle" font-size="7" fill="#f59e0b">scale(2, 0.6)</text>
  <text x="210" y="109" text-anchor="middle" font-size="6.5" fill="#94a3b8">ancho×2, alto×0.6</text>
</svg>
```

---

## El problema del origen: la escala mueve el elemento

Con `scale`, todo se escala desde el punto `(0,0)`. El elemento también se desplaza si no está en el origen.

```svg
<svg width="300" height="110"
     viewBox="0 0 300 110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">scale escala DESDE (0,0)</text>

  <!-- Original: rect en x=100, y=40 -->
  <rect x="100" y="40" width="40" height="35" fill="none" stroke="#94a3b8" stroke-width="1" stroke-dasharray="3,2"/>
  <text x="120" y="88" text-anchor="middle" font-size="7" fill="#94a3b8">x=100, y=40</text>

  <!-- scale(2): el rect aparece en x=200, y=80 (todo×2) -->
  <rect x="100" y="40" width="40" height="35" rx="2"
        fill="#fef2f2" stroke="#ef4444" stroke-width="1.5"
        transform="scale(2)"/>
  <text x="220" y="100" text-anchor="middle" font-size="7" fill="#ef4444">scale(2) → x=200, y=80</text>
  <text x="220" y="109" text-anchor="middle" font-size="6.5" fill="#94a3b8">todo×2, incluyendo la posición</text>
</svg>
```

---

## Escalar desde el centro propio: patrón translate/scale/translate

```svg
<svg width="300" height="110"
     viewBox="0 0 300 110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Centro del rect: cx=130, cy=55 -->
  <!-- Patrón: translate(cx,cy) scale(s) translate(-cx,-cy) -->
  <rect x="100" y="35" width="60" height="40" fill="none" stroke="#94a3b8" stroke-width="1" stroke-dasharray="3,2"/>
  <text x="130" y="28" text-anchor="middle" font-size="7" fill="#94a3b8">original</text>

  <!-- Escalado desde su propio centro -->
  <rect x="100" y="35" width="60" height="40" rx="3"
        fill="#dbeafe" stroke="#3b82f6" stroke-width="1.5"
        transform="translate(130,55) scale(1.6) translate(-130,-55)"/>
  <text x="130" y="92" text-anchor="middle" font-size="7" fill="#3b82f6">escala desde su centro</text>
  <text x="130" y="101" text-anchor="middle" font-size="6.5" fill="#94a3b8">translate(cx,cy) scale(1.6) translate(-cx,-cy)</text>
</svg>
```

---

## Valores negativos: espejo

```svg
<svg width="300" height="100"
     viewBox="0 0 300 100"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Flecha apuntando derecha (path original) -->
  <path d="M 5,15 L 20,8 L 20,12 L 30,12 L 30,18 L 20,18 L 20,22 Z"
        fill="#3b82f6"/>
  <text x="17" y="35" text-anchor="middle" font-size="7" fill="#64748b">original</text>

  <!-- Espejo horizontal: scale(-1,1) + translate para reposicionar -->
  <g transform="translate(120,0) scale(-1,1) translate(-35,0)">
    <path d="M 5,15 L 20,8 L 20,12 L 30,12 L 30,18 L 20,18 L 20,22 Z" fill="#ef4444"/>
  </g>
  <text x="100" y="35" text-anchor="middle" font-size="7" fill="#ef4444">scale(-1,1)</text>
  <text x="100" y="44" text-anchor="middle" font-size="6.5" fill="#94a3b8">espejo horizontal</text>

  <!-- Espejo vertical: scale(1,-1) -->
  <g transform="translate(175,45) scale(1,-1) translate(0,-22)">
    <path d="M 5,15 L 20,8 L 20,12 L 30,12 L 30,18 L 20,18 L 20,22 Z" fill="#10b981"/>
  </g>
  <text x="193" y="70" text-anchor="middle" font-size="7" fill="#10b981">scale(1,-1)</text>
  <text x="193" y="79" text-anchor="middle" font-size="6.5" fill="#94a3b8">espejo vertical</text>

  <!-- scale(-1,-1) = rotate(180) -->
  <g transform="translate(270,30) scale(-1,-1) translate(-18,-15)">
    <path d="M 5,15 L 20,8 L 20,12 L 30,12 L 30,18 L 20,18 L 20,22 Z" fill="#a855f7"/>
  </g>
  <text x="255" y="70" text-anchor="middle" font-size="7" fill="#a855f7">scale(-1,-1)</text>
  <text x="255" y="79" text-anchor="middle" font-size="6.5" fill="#94a3b8">180° (= rotate)</text>
</svg>
```
