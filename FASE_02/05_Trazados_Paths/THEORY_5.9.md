# 5.9 Sub-trazados (Sub-paths)

Un `<path>` puede contener múltiples partes independientes. Cada nuevo `M` inicia un sub-trazado desconectado del anterior.

---

## Qué es un sub-trazado

```svg
<svg width="300" height="120"
     viewBox="0 0 300 120"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Un solo <path> con 4 sub-trazados -->
  <path d="M 10 60 H 50
           M 70 60 H 110
           M 130 60 H 170
           M 190 60 H 230"
        stroke="#3b82f6" stroke-width="3" fill="none" stroke-linecap="round"/>

  <text x="150" y="90" text-anchor="middle" font-size="8" fill="#64748b">
    4 segmentos desconectados — 1 solo &lt;path&gt;
  </text>
  <text x="150" y="110" text-anchor="middle" font-size="7" fill="#94a3b8">
    Cada M inicia un sub-trazado nuevo
  </text>
</svg>
```

---

## Formas compuestas en un solo elemento

Un icono con partes separadas (puntos suspensivos, menú hamburguesa, la letra "i") es un solo `<path>`.

```svg
<svg width="300" height="150"
     viewBox="0 0 300 150"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#0f172a;display:block; font-family:sans-serif;">

  <!-- Menú hamburguesa: 3 líneas = 1 path -->
  <path d="M 20 35 H 80
           M 20 55 H 80
           M 20 75 H 80"
        stroke="#3b82f6" stroke-width="5" fill="none" stroke-linecap="round"/>
  <text x="50" y="100" text-anchor="middle" font-size="7.5" fill="#64748b">hamburguesa</text>
  <text x="50" y="112" text-anchor="middle" font-size="6.5" fill="#475569">1 path, 3 sub-trazados</text>

  <!-- Puntos suspensivos: 3 círculos = 1 path -->
  <path d="M 130 55 a 8 8 0 1 1 0.01 0
           M 165 55 a 8 8 0 1 1 0.01 0
           M 200 55 a 8 8 0 1 1 0.01 0"
        fill="#10b981"/>
  <text x="165" y="100" text-anchor="middle" font-size="7.5" fill="#64748b">puntos suspensivos</text>
  <text x="165" y="112" text-anchor="middle" font-size="6.5" fill="#475569">1 path, 3 sub-trazados</text>

  <!-- Letra "i": palo + punto = 1 path -->
  <path d="M 245 35 a 7 7 0 1 1 0.01 0
           M 245 55 V 95"
        fill="none" stroke="#f59e0b" stroke-width="5" stroke-linecap="round"/>
  <text x="250" y="125" text-anchor="middle" font-size="7.5" fill="#64748b">letra "i"</text>
  <text x="250" y="137" text-anchor="middle" font-size="6.5" fill="#475569">1 path, 2 sub-trazados</text>

  <text x="150" y="148" text-anchor="middle" font-size="7" fill="#334155">
    Iconos típicos = múltiples sub-trazados en un path
  </text>
</svg>
```

---

## Formas con agujeros: exterior + interior

Con `fill-rule="evenodd"`, el sub-trazado interior se vacía (ver sección 5.10).

```svg
<svg width="300" height="120"
     viewBox="0 0 300 120"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Anillo: círculo exterior + círculo interior (agujero) -->
  <path d="M 75 60 m -50 0 a 50 50 0 1 1 100 0 a 50 50 0 1 1 -100 0
           M 75 60 m -25 0 a 25 25 0 1 1 50 0 a 25 25 0 1 1 -50 0"
        fill="#3b82f6" fill-rule="evenodd" opacity="0.85"/>
  <text x="75" y="110" text-anchor="middle" font-size="7.5" fill="#64748b">anillo (evenodd)</text>

  <!-- Marco: rectángulo exterior + interior -->
  <path d="M 130 20 H 260 V 100 H 130 Z
           M 145 35 H 245 V 85 H 145 Z"
        fill="#10b981" fill-rule="evenodd" opacity="0.8"/>
  <text x="195" y="115" text-anchor="middle" font-size="7.5" fill="#64748b">marco (evenodd)</text>

  <!-- Sin evenodd: ambos rellenos -->
  <path d="M 280 60 m -38 0 a 38 38 0 1 1 76 0 a 38 38 0 1 1 -76 0
           M 280 60 m -18 0 a 18 18 0 1 1 36 0 a 18 18 0 1 1 -36 0"
        fill="#f59e0b" fill-rule="nonzero" opacity="0.8"/>
  <text x="280" y="110" text-anchor="middle" font-size="7" fill="#64748b">nonzero</text>
  <text x="280" y="120" text-anchor="middle" font-size="6.5" fill="#94a3b8">(sin agujero)</text>
</svg>
```

---

## Ventaja de rendimiento

Un `<path>` con múltiples sub-trazados es **más eficiente** que múltiples elementos `<path>` separados:

```svg
<svg width="300" height="90"
     viewBox="0 0 300 90"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">1 path vs N paths</text>

  <!-- Malo: múltiples elementos -->
  <rect x="5"  y="28" width="135" height="55" rx="3" fill="#fef2f2" stroke="#fecaca" stroke-width="1"/>
  <text x="72" y="44" text-anchor="middle" font-size="7.5" fill="#ef4444">3 elementos &lt;path&gt;</text>
  <text x="72" y="56" text-anchor="middle" font-size="7" fill="#64748b">= 3 nodos en el DOM</text>
  <text x="72" y="68" text-anchor="middle" font-size="7" fill="#64748b">= 3× gestión de eventos</text>
  <text x="72" y="80" text-anchor="middle" font-size="7" fill="#64748b">= 3× atributos repetidos</text>

  <!-- Bueno: un elemento con sub-trazados -->
  <rect x="155" y="28" width="140" height="55" rx="3" fill="#f0fdf4" stroke="#bbf7d0" stroke-width="1"/>
  <text x="225" y="44" text-anchor="middle" font-size="7.5" fill="#10b981">1 &lt;path&gt; con 3 M ✓</text>
  <text x="225" y="56" text-anchor="middle" font-size="7" fill="#64748b">= 1 nodo en el DOM</text>
  <text x="225" y="68" text-anchor="middle" font-size="7" fill="#64748b">= fill/stroke heredados</text>
  <text x="225" y="80" text-anchor="middle" font-size="7" fill="#64748b">= más eficiente</text>
</svg>
```
