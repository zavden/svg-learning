# 5.1 Introducción a `<path>`

`<path>` es el elemento más poderoso de SVG. Puede reproducir cualquier forma que las primitivas básicas pueden hacer — y formas que ninguna primitiva puede hacer.

---

## ¿Por qué existe `<path>`?

Las formas básicas son atajos para geometría común. `<path>` es el lenguaje universal que describe cualquier forma posible.

```svg
<svg width="300" height="130"
     viewBox="0 0 300 130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- <rect> vs su equivalente <path> -->
  <rect x="20" y="30" width="80" height="60" fill="#3b82f6" rx="0"/>
  <text x="60" y="108" text-anchor="middle" font-size="8" fill="#64748b">&lt;rect&gt;</text>

  <text x="150" y="65" text-anchor="middle" font-size="18" fill="#94a3b8">=</text>

  <path d="M 200 30 H 280 V 90 H 200 Z" fill="#3b82f6"/>
  <text x="240" y="108" text-anchor="middle" font-size="8" fill="#64748b">&lt;path&gt;</text>

  <text x="150" y="125" text-anchor="middle" font-size="7" fill="#94a3b8">
    Internamente el navegador puede tratar ambos igual
  </text>
</svg>
```

---

## La metáfora del lápiz

Imagina un lápiz sobre papel. Los comandos de `<path>` controlan ese lápiz:

```svg
<svg width="300" height="160"
     viewBox="0 0 300 160"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#0f172a;display:block; font-family:sans-serif;">

  <!-- Fondo de "papel" -->
  <rect x="10" y="10" width="280" height="140" rx="6" fill="#1e293b"/>

  <!-- Path dibujado -->
  <path d="M 40 120 L 90 50 L 140 100 C 160 30 200 30 220 80 A 30 30 0 0 1 270 80"
        stroke="#3b82f6" stroke-width="2.5" fill="none" stroke-linecap="round" stroke-linejoin="round"/>

  <!-- Marcas de puntos clave -->
  <circle cx="40"  cy="120" r="4" fill="#ef4444"/>
  <circle cx="90"  cy="50"  r="4" fill="#f59e0b"/>
  <circle cx="140" cy="100" r="4" fill="#f59e0b"/>
  <circle cx="220" cy="80"  r="4" fill="#10b981"/>
  <circle cx="270" cy="80"  r="4" fill="#a855f7"/>

  <!-- Etiquetas -->
  <text x="30"  y="138" font-size="7" fill="#ef4444">M (inicio)</text>
  <text x="75"  y="45"  font-size="7" fill="#f59e0b">L (línea)</text>
  <text x="144" y="115" font-size="7" fill="#f59e0b">C (curva)</text>
  <text x="223" y="98"  font-size="7" fill="#10b981">A (arco)</text>
  <text x="256" y="95"  font-size="7" fill="#a855f7">fin</text>

  <text x="150" y="152" text-anchor="middle" font-size="7" fill="#475569">
    M, L, C, A — comandos del "lápiz"
  </text>
</svg>
```

---

## Tabla de todos los comandos

| Cmd | Nombre | Parámetros | Descripción |
|-----|--------|------------|-------------|
| `M`/`m` | Move To | x y | Mueve sin dibujar |
| `L`/`l` | Line To | x y | Línea recta |
| `H`/`h` | Horizontal | x | Línea horizontal |
| `V`/`v` | Vertical | y | Línea vertical |
| `Z`/`z` | Close Path | — | Cierra al punto inicial |
| `C`/`c` | Cubic Bézier | x1 y1, x2 y2, x y | Curva cúbica |
| `S`/`s` | Smooth Cubic | x2 y2, x y | Cúbica suave |
| `Q`/`q` | Quadratic Bézier | x1 y1, x y | Curva cuadrática |
| `T`/`t` | Smooth Quad | x y | Cuadrática suave |
| `A`/`a` | Arc | rx ry rot large sweep x y | Arco de elipse |

**MAYÚSCULA = absoluto** (coordenadas desde el origen). **Minúscula = relativo** (desde la posición actual).

---

## Lo que `<path>` puede hacer que las primitivas no pueden

```svg
<svg width="300" height="140"
     viewBox="0 0 300 140"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Flecha -->
  <path d="M 10 55 L 50 55 L 50 40 L 75 65 L 50 90 L 50 75 L 10 75 Z"
        fill="#3b82f6"/>
  <text x="42" y="108" text-anchor="middle" font-size="7.5" fill="#64748b">flecha</text>

  <!-- Burbuja de diálogo -->
  <path d="M 95 20 Q 95 10 105 10 L 185 10 Q 195 10 195 20 L 195 70 Q 195 80 185 80 L 140 80 L 128 95 L 125 80 L 105 80 Q 95 80 95 70 Z"
        fill="#10b981" opacity="0.85"/>
  <text x="145" y="108" text-anchor="middle" font-size="7.5" fill="#64748b">burbuja</text>

  <!-- Forma orgánica / blob -->
  <path d="M 220 65 C 215 30 245 20 255 40 C 270 55 285 45 280 65 C 290 85 270 100 255 90 C 240 105 215 95 220 65 Z"
        fill="#a855f7" opacity="0.85"/>
  <text x="252" y="118" text-anchor="middle" font-size="7.5" fill="#64748b">blob</text>

  <text x="150" y="132" text-anchor="middle" font-size="7" fill="#94a3b8">
    Ninguna forma básica puede hacer esto
  </text>
</svg>
```
