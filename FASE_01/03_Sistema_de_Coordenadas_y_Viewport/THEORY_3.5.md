# 3.5 Sistemas de coordenadas anidados

SVG permite crear **nuevos sistemas de coordenadas** dentro de un sistema existente. Esto ocurre de dos formas principales:

1. **SVG anidado (`<svg>` dentro de `<svg>`)**: el elemento `<svg>` hijo crea su propio viewport y, si tiene `viewBox`, su propio sistema de coordenadas independiente.
2. **Transformaciones (`transform="..."` en `<g>` u otros elementos)**: las transformaciones crean un sistema de coordenadas local para todos los elementos dentro del grupo.

Entender los sistemas anidados es clave para trabajar con componentes SVG reutilizables, animaciones complejas y cualquier gráfico que combine partes con distintas escalas o proporciones.

---

## Ejemplo 1: SVG dentro de SVG — el hijo crea su propio viewport

Un `<svg>` hijo se posiciona con `x` e `y` en el espacio de coordenadas del padre. Sus atributos `width` y `height` definen su tamaño en ese espacio. Sin `viewBox` propio, sus coordenadas internas continúan siendo las mismas que las del padre.

```svg
<svg width="300" height="200"
     style="border:2px solid #e2e8f0;background:#f0f4f8;display:block;">
  <!-- SVG padre: espacio de coordenadas 300×200 -->
  <text x="150" y="16" text-anchor="middle"
        font-size="10" fill="#64748b">SVG padre (300×200)</text>

  <!-- SVG hijo 1: posicionado en (20, 30), tamaño 100×80 -->
  <!-- Sus coordenadas internas empiezan en (0,0) dentro de él -->
  <svg x="20" y="30" width="100" height="80"
       style="border:2px solid #3b82f6;background:#eff6ff;overflow:visible;">
    <!-- Coord (0,0) es la esquina sup-izq de ESTE svg hijo, no del padre -->
    <circle cx="50" cy="40" r="30" fill="#3b82f6" opacity="0.8"/>
    <circle cx="50" cy="40" r="4"  fill="white"/>
    <text x="50" y="44" text-anchor="middle"
          font-size="9" fill="white">hijo 1</text>
    <!-- Origen local del hijo -->
    <circle cx="0" cy="0" r="4" fill="tomato"/>
    <text x="3" y="12" font-size="8" fill="tomato">(0,0)</text>
  </svg>
  <text x="70" y="125" text-anchor="middle"
        font-size="9" fill="#3b82f6">x=20, y=30 (en padre)</text>

  <!-- SVG hijo 2: posicionado en (160, 50), tamaño 120×100 -->
  <svg x="160" y="50" width="120" height="100"
       style="border:2px solid #10b981;background:#f0fdf4;overflow:visible;">
    <circle cx="60" cy="50" r="40" fill="#10b981" opacity="0.8"/>
    <circle cx="60" cy="50" r="4"  fill="white"/>
    <text x="60" y="54" text-anchor="middle"
          font-size="9" fill="white">hijo 2</text>
    <circle cx="0" cy="0" r="4" fill="tomato"/>
    <text x="3" y="12" font-size="8" fill="tomato">(0,0)</text>
  </svg>
  <text x="220" y="165" text-anchor="middle"
        font-size="9" fill="#10b981">x=160, y=50 (en padre)</text>

  <!-- Origen del padre -->
  <circle cx="0" cy="0" r="5" fill="tomato"/>
  <text x="5" y="16" font-size="9" fill="tomato">(0,0) padre</text>
</svg>
```

---

## Ejemplo 2: SVG hijo con `viewBox` propio — coordenadas independientes

Cuando el `<svg>` hijo tiene su propio `viewBox`, sus elementos usan un sistema de coordenadas completamente diferente al del padre. El mismo contenido puede dibujarse con coordenadas de 0-24 aunque el hijo ocupe 120×120 píxeles en el padre.

```svg
<svg width="300" height="220"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block;">
  <!-- Padre: coordenadas 0-300 horizontal, 0-220 vertical -->
  <text x="150" y="18" text-anchor="middle"
        font-size="10" fill="#475569">SVG padre (300×220)</text>

  <!-- Hijo izquierdo: ocupa 120×120 en el padre pero usa coords 0-24 -->
  <!-- Es como un ícono SVG estándar: tamaño interno fijo, escala libre -->
  <text x="80" y="40" text-anchor="middle"
        font-size="9" fill="#3b82f6">hijo: viewBox="0 0 24 24"</text>
  <svg x="20" y="48" width="120" height="120"
       viewBox="0 0 24 24"
       style="border:2px solid #3b82f6;background:#eff6ff;">
    <!-- Coordenadas aquí son 0-24, aunque el svg mide 120×120 en el padre -->
    <!-- Un elemento en x=12 está exactamente en el centro (12/24 = 50%) -->
    <circle cx="12" cy="12" r="10" fill="#3b82f6" opacity="0.8"/>
    <circle cx="8"  cy="10" r="1.5" fill="white"/>
    <circle cx="16" cy="10" r="1.5" fill="white"/>
    <path d="M 7 15 Q 12 20 17 15"
          fill="none" stroke="white"
          stroke-width="1.5" stroke-linecap="round"/>
    <!-- En este viewBox, (0,0) es la esquina sup-izq y (24,24) la inf-der -->
    <circle cx="0" cy="0" r="1" fill="tomato"/>
    <text x="1" y="4" font-size="3" fill="tomato">(0,0)</text>
    <text x="12" y="23" text-anchor="middle"
          font-size="2.5" fill="#1e40af">coords 0-24</text>
  </svg>

  <!-- Hijo derecho: ocupa 100×160 en el padre y usa coords 0-100 -->
  <text x="220" y="40" text-anchor="middle"
        font-size="9" fill="#10b981">hijo: viewBox="0 0 100 160"</text>
  <svg x="165" y="48" width="110" height="160"
       viewBox="0 0 100 160"
       style="border:2px solid #10b981;background:#f0fdf4;">
    <!-- Coordenadas aquí son 0-100 × 0-160 -->
    <rect x="10" y="10" width="80" height="140"
          fill="#d1fae5" rx="5"/>
    <circle cx="50" cy="60" r="35" fill="#10b981" opacity="0.8"/>
    <circle cx="38" cy="52" r="5"  fill="white"/>
    <circle cx="62" cy="52" r="5"  fill="white"/>
    <path d="M 32 72 Q 50 88 68 72"
          fill="none" stroke="white"
          stroke-width="4" stroke-linecap="round"/>
    <text x="50" y="130" text-anchor="middle"
          font-size="10" fill="#065f46">coords 0-100</text>
    <circle cx="0" cy="0" r="2" fill="tomato"/>
    <text x="2" y="8" font-size="5" fill="tomato">(0,0)</text>
  </svg>

  <text x="150" y="212" text-anchor="middle"
        font-size="9" fill="#94a3b8">
    Cada hijo con viewBox tiene su propio sistema de coordenadas
  </text>
</svg>
```

---

## Ejemplo 3: `<g>` con `transform="translate()"` — nuevo origen local

El elemento `<g>` con `transform="translate(tx, ty)"` desplaza el origen del sistema de coordenadas para todos sus hijos. En lugar de calcular posiciones absolutas, los elementos del grupo pueden usar coordenadas relativas a su posición local.

```svg
<svg width="280" height="200"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block;">
  <!-- Sin transform: todos los elementos usan coordenadas globales -->
  <!-- Con translate(x, y): los hijos usan coords relativas al nuevo origen -->

  <!-- === Sin transform (referencias globales) === -->
  <text x="60" y="14" text-anchor="middle"
        font-size="10" fill="#64748b">Sin transform</text>
  <!-- Cara dibujada con coords absolutas desde (0,0) global -->
  <circle cx="60" cy="80" r="40" fill="#e2e8f0"/>
  <circle cx="47" cy="70" r="5"  fill="#475569"/>
  <circle cx="73" cy="70" r="5"  fill="#475569"/>
  <path d="M 44 93 Q 60 108 76 93"
        fill="none" stroke="#475569"
        stroke-width="3" stroke-linecap="round"/>
  <text x="60" y="135" text-anchor="middle"
        font-size="8" fill="#94a3b8">cx=60, cy=80, etc.</text>

  <!-- Separador -->
  <line x1="130" y1="10" x2="130" y2="190"
        stroke="#e2e8f0" stroke-width="1"/>

  <!-- === Con translate (el grupo tiene su propio origen) === -->
  <text x="210" y="14" text-anchor="middle"
        font-size="10" fill="#3b82f6">translate(190, 80)</text>

  <!-- translate(190, 80) mueve el origen a (190, 80) del SVG padre -->
  <!-- Los hijos usan coordenadas RELATIVAS a ese nuevo origen -->
  <g transform="translate(190, 80)">
    <!-- Aquí (0,0) es el CENTRO DE LA CARA, no el origen del SVG padre -->
    <!-- Las mismas formas, pero con coords mucho más simples -->
    <circle cx="0"   cy="0"  r="40" fill="#dbeafe"/>
    <circle cx="-13" cy="-10" r="5" fill="#3b82f6"/>
    <circle cx="13"  cy="-10" r="5" fill="#3b82f6"/>
    <path d="M -16 13 Q 0 28 16 13"
          fill="none" stroke="#3b82f6"
          stroke-width="3" stroke-linecap="round"/>

    <!-- Origen local del grupo: (0,0) respecto al translate -->
    <circle cx="0" cy="0" r="4" fill="tomato"/>
    <text x="4" y="-2" font-size="7" fill="tomato">(0,0) local</text>
  </g>
  <text x="210" y="135" text-anchor="middle"
        font-size="8" fill="#3b82f6">cx=0, cy=0 (coords locales)</text>
</svg>
```

---

## Ejemplo 4: Transformaciones anidadas — los sistemas se acumulan

Cuando hay `<g>` anidados, cada `transform` se **suma** al anterior. El sistema de coordenadas se compone: un grupo trasladado que contiene otro grupo trasladado lleva los elementos a la suma de ambas traslaciones.

```svg
<svg width="280" height="220"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block;">
  <!-- Las transformaciones anidadas se acumulan -->
  <!-- Sistema de referencia: (0,0) global en la esquina sup-izquierda -->

  <!-- Origen global para referencia -->
  <circle cx="0" cy="0" r="5" fill="tomato"/>
  <text x="5" y="14" font-size="9" fill="tomato">(0,0) global</text>

  <!-- Nivel 1: translate(60, 50) -->
  <!-- Nuevo origen en (60, 50) del SVG padre -->
  <g transform="translate(60, 50)">
    <circle cx="0" cy="0" r="5" fill="#3b82f6"/>
    <text x="4" y="-2" font-size="8" fill="#3b82f6">translate(60,50)</text>
    <rect x="-50" y="-10" width="100" height="80"
          fill="none" stroke="#3b82f6" stroke-width="1"
          stroke-dasharray="4,2"/>

    <!-- Nivel 2: translate(80, 30) RELATIVO al nivel 1 -->
    <!-- En coords globales: (60+80, 50+30) = (140, 80) -->
    <g transform="translate(80, 30)">
      <circle cx="0" cy="0" r="5" fill="#10b981"/>
      <text x="4" y="-2" font-size="8" fill="#10b981">
        translate(80,30) → global: (140,80)
      </text>
      <rect x="-45" y="-10" width="90" height="70"
            fill="none" stroke="#10b981" stroke-width="1"
            stroke-dasharray="4,2"/>

      <!-- Nivel 3: translate(60, 40) RELATIVO al nivel 2 -->
      <!-- En coords globales: (140+60, 80+40) = (200, 120) -->
      <g transform="translate(60, 40)">
        <circle cx="0" cy="0" r="5" fill="#f59e0b"/>
        <text x="4" y="-2" font-size="8" fill="#f59e0b">
          translate(60,40) → global: (200,120)
        </text>
        <circle cx="0" cy="0" r="25" fill="#f59e0b" opacity="0.3"/>
      </g>
    </g>
  </g>

  <text x="140" y="200" text-anchor="middle"
        font-size="9" fill="#64748b">
    Cada translate se suma al anterior: los sistemas se acumulan
  </text>
</svg>
```

---

## Ejemplo 5: `rotate()` en un grupo local — rotación alrededor del origen local

Sin coordinar el centro de rotación, `rotate()` gira alrededor del (0,0) del SVG padre. Usando `translate` primero se lleva el objeto al origen, luego se rota, y luego se vuelve a colocar. O directamente: `rotate(angle, cx, cy)`.

```svg
<svg width="300" height="220"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block;">
  <!-- Comparación: rotate() sin punto de pivote vs con punto de pivote -->

  <!-- Etiquetas -->
  <text x="75" y="16" text-anchor="middle"
        font-size="10" fill="#475569">rotate(30) — pivote en (0,0)</text>
  <text x="225" y="16" text-anchor="middle"
        font-size="10" fill="#3b82f6">rotate(30,75,110) — pivote propio</text>
  <line x1="150" y1="12" x2="150" y2="210"
        stroke="#e2e8f0" stroke-width="1"/>

  <!-- === rotate sin especificar pivote → gira alrededor de (0,0) global === -->
  <!-- El cuadrado se va hacia el extremo por la rotación alrededor del origen -->
  <g transform="rotate(30)">
    <rect x="35" y="80" width="70" height="50"
          fill="#e2e8f0" stroke="#94a3b8" stroke-width="2" rx="3"/>
    <text x="70" y="110" text-anchor="middle"
          font-size="9" fill="#64748b">rotado</text>
  </g>
  <!-- Posición original para comparar -->
  <rect x="35" y="80" width="70" height="50"
        fill="none" stroke="#cbd5e1"
        stroke-width="1" stroke-dasharray="4,2" rx="3"/>
  <!-- El pivote de rotación -->
  <circle cx="0" cy="0" r="5" fill="tomato"/>
  <text x="4" y="14" font-size="8" fill="tomato">pivote (0,0)</text>

  <!-- === rotate con cx, cy → gira alrededor del centro del objeto === -->
  <!-- rotate(angle, cx, cy): gira 30° alrededor del punto (225, 110) -->
  <g transform="rotate(30, 225, 110)">
    <rect x="185" y="80" width="80" height="60"
          fill="#dbeafe" stroke="#3b82f6" stroke-width="2" rx="3"/>
    <text x="225" y="114" text-anchor="middle"
          font-size="9" fill="#3b82f6">rotado</text>
  </g>
  <!-- Posición original para comparar -->
  <rect x="185" y="80" width="80" height="60"
        fill="none" stroke="#93c5fd"
        stroke-width="1" stroke-dasharray="4,2" rx="3"/>
  <!-- El pivote está en el centro del rectángulo -->
  <circle cx="225" cy="110" r="5" fill="tomato"/>
  <text x="229" y="107" font-size="8" fill="tomato">pivote (225,110)</text>

  <text x="150" y="200" text-anchor="middle"
        font-size="9" fill="#64748b">
    rotate(deg, cx, cy) especifica el punto de pivote
  </text>
  <text x="150" y="214" text-anchor="middle"
        font-size="9" fill="#94a3b8">
    Sin cx,cy el pivote es (0,0) global — resultado inesperado
  </text>
</svg>
```
