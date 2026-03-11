# Referencia rápida — Trazados (Paths)

---

## Cheat sheet: todos los comandos

```svg
<svg width="300" height="250"
     viewBox="0 0 300 250"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#0f172a;display:block; font-family:sans-serif;">

  <!-- Cabecera -->
  <rect x="0" y="0" width="300" height="22" fill="#1e3a5f"/>
  <text x="150" y="15" text-anchor="middle" font-size="9" fill="#93c5fd">Comandos de &lt;path&gt;</text>

  <!-- M -->
  <text x="10" y="37" font-size="8.5" fill="#60a5fa" font-family="monospace">M/m x y</text>
  <text x="110" y="37" font-size="7.5" fill="#94a3b8">Move To — mueve sin dibujar</text>

  <!-- L -->
  <text x="10" y="52" font-size="8.5" fill="#34d399" font-family="monospace">L/l x y</text>
  <text x="110" y="52" font-size="7.5" fill="#94a3b8">Line To — línea recta</text>

  <!-- H -->
  <text x="10" y="67" font-size="8.5" fill="#34d399" font-family="monospace">H/h x</text>
  <text x="110" y="67" font-size="7.5" fill="#94a3b8">Horizontal Line — solo X</text>

  <!-- V -->
  <text x="10" y="82" font-size="8.5" fill="#34d399" font-family="monospace">V/v y</text>
  <text x="110" y="82" font-size="7.5" fill="#94a3b8">Vertical Line — solo Y</text>

  <!-- Z -->
  <text x="10" y="97" font-size="8.5" fill="#fbbf24" font-family="monospace">Z/z</text>
  <text x="110" y="97" font-size="7.5" fill="#94a3b8">Close Path — regresa al M</text>

  <!-- C -->
  <text x="10" y="115" font-size="8.5" fill="#c084fc" font-family="monospace">C/c x1 y1,x2 y2,x y</text>
  <text x="10" y="127" font-size="7" fill="#94a3b8">Cubic Bézier — 2 puntos de control</text>

  <!-- S -->
  <text x="10" y="142" font-size="8.5" fill="#c084fc" font-family="monospace">S/s x2 y2,x y</text>
  <text x="10" y="154" font-size="7" fill="#94a3b8">Smooth Cubic — CP1 automático</text>

  <!-- Q -->
  <text x="10" y="169" font-size="8.5" fill="#fb923c" font-family="monospace">Q/q x1 y1,x y</text>
  <text x="10" y="181" font-size="7" fill="#94a3b8">Quadratic Bézier — 1 punto de control</text>

  <!-- T -->
  <text x="10" y="196" font-size="8.5" fill="#fb923c" font-family="monospace">T/t x y</text>
  <text x="10" y="208" font-size="7" fill="#94a3b8">Smooth Quad — CP automático</text>

  <!-- A -->
  <text x="10" y="223" font-size="8.5" fill="#f87171" font-family="monospace">A/a rx ry rot la sw x y</text>
  <text x="10" y="235" font-size="7" fill="#94a3b8">Arc — fragmento de elipse (7 params)</text>

  <!-- Nota -->
  <text x="150" y="248" text-anchor="middle" font-size="6.5" fill="#475569">
    MAYÚSCULA = absoluto | minúscula = relativo
  </text>
</svg>
```

---

## Los 4 arcos posibles (`A`)

```svg
<svg width="300" height="130"
     viewBox="0 0 300 130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">A rx ry 0 large sweep x y</text>

  <!-- Punto inicio y fin -->
  <circle cx="80"  cy="75" r="4" fill="#ef4444"/>
  <circle cx="220" cy="75" r="4" fill="#ef4444"/>

  <!-- 0 0: pequeño, antihorario -->
  <path d="M 80 75 A 80 40 0 0 0 220 75" stroke="#3b82f6" stroke-width="2" fill="none"/>
  <text x="150" y="28" text-anchor="middle" font-size="7.5" fill="#3b82f6">large=0, sweep=0</text>

  <!-- 0 1: pequeño, horario -->
  <path d="M 80 75 A 80 40 0 0 1 220 75" stroke="#10b981" stroke-width="2" fill="none"/>
  <text x="150" y="126" text-anchor="middle" font-size="7.5" fill="#10b981">large=0, sweep=1</text>

  <!-- 1 0: grande, antihorario (punteado) -->
  <path d="M 80 75 A 80 40 0 1 0 220 75" stroke="#f59e0b" stroke-width="1.5" fill="none" stroke-dasharray="4,2"/>
  <text x="20"  y="75" text-anchor="middle" font-size="6.5" fill="#f59e0b">1,0</text>

  <!-- 1 1: grande, horario (punteado) -->
  <path d="M 80 75 A 80 40 0 1 1 220 75" stroke="#a855f7" stroke-width="1.5" fill="none" stroke-dasharray="4,2"/>
  <text x="280" y="75" text-anchor="middle" font-size="6.5" fill="#a855f7">1,1</text>
</svg>
```

---

## Patrones frecuentes de path

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Flecha derecha -->
  <path d="M 10 45 L 50 45 L 50 30 L 75 55 L 50 80 L 50 65 L 10 65 Z"
        fill="#3b82f6" opacity="0.85"/>
  <text x="42" y="95" text-anchor="middle" font-size="7.5" fill="#64748b">flecha</text>

  <!-- Chevron -->
  <path d="M 105 35 L 135 55 L 105 75" stroke="#10b981" stroke-width="4" fill="none" stroke-linecap="round" stroke-linejoin="round"/>
  <text x="120" y="95" text-anchor="middle" font-size="7.5" fill="#64748b">chevron</text>

  <!-- Check -->
  <path d="M 155 60 L 170 75 L 200 35" stroke="#f59e0b" stroke-width="4" fill="none" stroke-linecap="round" stroke-linejoin="round"/>
  <text x="178" y="95" text-anchor="middle" font-size="7.5" fill="#64748b">check</text>

  <!-- X (cerrar) -->
  <path d="M 220 35 L 260 75 M 260 35 L 220 75" stroke="#ef4444" stroke-width="4" fill="none" stroke-linecap="round"/>
  <text x="240" y="95" text-anchor="middle" font-size="7.5" fill="#64748b">cerrar</text>

  <!-- Onda -->
  <path d="M 10 140 C 30 110, 60 170, 80 140 S 130 110, 150 140 S 200 170, 220 140"
        stroke="#6366f1" stroke-width="2.5" fill="none"/>
  <text x="115" y="165" text-anchor="middle" font-size="7.5" fill="#64748b">onda (C + S + S)</text>

  <!-- Burbuja de diálogo -->
  <path d="M 10 185 Q 10 175 20 175 L 100 175 Q 110 175 110 185 L 110 205 Q 110 215 100 215 L 60 215 L 50 220 L 48 215 L 20 215 Q 10 215 10 205 Z"
        fill="#a855f7" opacity="0.85"/>
  <text x="60" y="213" text-anchor="middle" font-size="7" fill="white">Hola!</text>
  <text x="60" y="230" text-anchor="middle" font-size="7.5" fill="#64748b">burbuja (Q + cola)</text>

  <!-- Progress ring (75%) -->
  <circle cx="210" cy="200" r="28" fill="none" stroke="#e2e8f0" stroke-width="8"/>
  <path d="M 210 172 A 28 28 0 1 1 188 221.2"
        stroke="#3b82f6" stroke-width="8" fill="none" stroke-linecap="round"/>
  <text x="210" y="204" text-anchor="middle" font-size="9" fill="#64748b" font-weight="700">75%</text>
  <text x="210" y="230" text-anchor="middle" font-size="7.5" fill="#64748b">progress ring</text>
</svg>
```
