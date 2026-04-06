# 15.2 Aplicación de marcadores

Una vez definido el `<marker>`, se aplica a una línea, polilínea o path mediante tres atributos: `marker-start`, `marker-mid` y `marker-end`. Cada uno controla una posición distinta del camino.

---

## Los tres puntos: start, mid, end

```svg
<svg width="300" height="200"
     viewBox="0 0 300 200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">marker-start, marker-mid, marker-end</text>

  <defs>
    <marker id="m-start" viewBox="0 0 10 10" refX="5" refY="5"
            markerWidth="6" markerHeight="6">
      <circle cx="5" cy="5" r="4" fill="#10b981"/>
    </marker>
    <marker id="m-mid" viewBox="0 0 10 10" refX="5" refY="5"
            markerWidth="6" markerHeight="6">
      <circle cx="5" cy="5" r="4" fill="#f59e0b"/>
    </marker>
    <marker id="m-end" viewBox="0 0 10 10" refX="5" refY="5"
            markerWidth="6" markerHeight="6">
      <circle cx="5" cy="5" r="4" fill="#ef4444"/>
    </marker>
  </defs>

  <!-- Polyline con 5 puntos → 1 start + 3 mid + 1 end -->
  <polyline points="30,100 90,60 150,110 210,70 270,100"
            fill="none" stroke="#94a3b8" stroke-width="1.5"
            marker-start="url(#m-start)"
            marker-mid="url(#m-mid)"
            marker-end="url(#m-end)"/>

  <!-- Leyenda -->
  <circle cx="60" cy="155" r="5" fill="#10b981"/>
  <text x="70" y="158" font-size="7" fill="#1e293b">start (primer punto)</text>

  <circle cx="155" cy="155" r="5" fill="#f59e0b"/>
  <text x="165" y="158" font-size="7" fill="#1e293b">mid (vértices)</text>

  <circle cx="245" cy="155" r="5" fill="#ef4444"/>
  <text x="255" y="158" font-size="7" fill="#1e293b">end</text>

  <text x="150" y="185" text-anchor="middle" font-size="7" fill="#94a3b8">
    polyline de 5 puntos: 1 verde + 3 naranjas + 1 rojo
  </text>
</svg>
```

---

## `marker` como atajo: mismo marcador en todas las posiciones

```svg
<svg width="300" height="180"
     viewBox="0 0 300 180"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">marker="url(#id)" = start + mid + end</text>

  <defs>
    <marker id="dot" viewBox="0 0 10 10" refX="5" refY="5"
            markerWidth="5" markerHeight="5">
      <circle cx="5" cy="5" r="4" fill="#3b82f6"/>
    </marker>
  </defs>

  <!-- Versión larga: tres atributos -->
  <text x="150" y="40" text-anchor="middle" font-size="7" font-weight="bold" fill="#1e293b">
    versión explícita
  </text>
  <polyline points="30,60 90,50 150,75 210,45 270,65"
            fill="none" stroke="#94a3b8" stroke-width="1.5"
            marker-start="url(#dot)"
            marker-mid="url(#dot)"
            marker-end="url(#dot)"/>

  <!-- Versión atajo -->
  <text x="150" y="105" text-anchor="middle" font-size="7" font-weight="bold" fill="#1e293b">
    versión con marker="..."
  </text>
  <polyline points="30,125 90,115 150,140 210,110 270,130"
            fill="none" stroke="#94a3b8" stroke-width="1.5"
            marker="url(#dot)"/>

  <text x="150" y="165" text-anchor="middle" font-size="7" fill="#94a3b8">
    útil cuando quieres el mismo marker en los tres sitios
  </text>
</svg>
```

---

## Distinto marcador en cada extremo

```svg
<svg width="300" height="200"
     viewBox="0 0 300 200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">inicio y final con distinto símbolo</text>

  <defs>
    <!-- Círculo relleno para el inicio -->
    <marker id="t-start" viewBox="0 0 10 10" refX="5" refY="5"
            markerWidth="7" markerHeight="7">
      <circle cx="5" cy="5" r="4" fill="#10b981"/>
    </marker>
    <!-- Flecha para el final -->
    <marker id="t-arrow" viewBox="0 0 10 10" refX="9" refY="5"
            markerWidth="7" markerHeight="7" orient="auto">
      <path d="M 0 0 L 10 5 L 0 10 z" fill="#3b82f6"/>
    </marker>
    <!-- Cuadrado para el final alterno -->
    <marker id="t-square" viewBox="0 0 10 10" refX="5" refY="5"
            markerWidth="7" markerHeight="7">
      <rect x="1" y="1" width="8" height="8" fill="#ec4899"/>
    </marker>
    <!-- Flecha hueca -->
    <marker id="t-hollow" viewBox="0 0 10 10" refX="9" refY="5"
            markerWidth="8" markerHeight="8" orient="auto">
      <path d="M 0 0 L 10 5 L 0 10 z" fill="white" stroke="#f59e0b" stroke-width="1.5"/>
    </marker>
  </defs>

  <!-- Línea 1: círculo → flecha -->
  <line x1="30" y1="55" x2="270" y2="55" stroke="#1e293b" stroke-width="1.5"
        marker-start="url(#t-start)" marker-end="url(#t-arrow)"/>
  <text x="30" y="75" font-size="7" fill="#64748b">círculo (start) → flecha (end)</text>

  <!-- Línea 2: cuadrado → flecha hueca -->
  <line x1="30" y1="105" x2="270" y2="105" stroke="#1e293b" stroke-width="1.5"
        marker-start="url(#t-square)" marker-end="url(#t-hollow)"/>
  <text x="30" y="125" font-size="7" fill="#64748b">cuadrado → flecha hueca</text>

  <!-- Línea 3: flecha → flecha hueca (relación asimétrica) -->
  <line x1="30" y1="155" x2="270" y2="155" stroke="#1e293b" stroke-width="1.5"
        marker-start="url(#t-arrow)" marker-end="url(#t-hollow)"/>
  <text x="30" y="175" font-size="7" fill="#64748b">flecha (start) → flecha hueca (end)</text>

  <text x="150" y="195" text-anchor="middle" font-size="6" fill="#94a3b8">
    el start no rota automáticamente — requiere orient="auto-start-reverse"
  </text>
</svg>
```

---

## `orient="auto-start-reverse"`: un marker para ambos extremos

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">auto-start-reverse: flecha bidireccional</text>

  <defs>
    <!-- orient="auto": el start apunta en la dirección errónea -->
    <marker id="ar-auto" viewBox="0 0 10 10" refX="9" refY="5"
            markerWidth="7" markerHeight="7" orient="auto">
      <path d="M 0 0 L 10 5 L 0 10 z" fill="#ef4444"/>
    </marker>
    <!-- orient="auto-start-reverse": el start se voltea automáticamente -->
    <marker id="ar-rev" viewBox="0 0 10 10" refX="9" refY="5"
            markerWidth="7" markerHeight="7" orient="auto-start-reverse">
      <path d="M 0 0 L 10 5 L 0 10 z" fill="#10b981"/>
    </marker>
  </defs>

  <!-- MAL: orient="auto" → las puntas miran al mismo lado -->
  <text x="150" y="45" text-anchor="middle" font-size="8" font-weight="bold" fill="#991b1b">
    orient="auto" · misma flecha en start y end
  </text>
  <line x1="50" y1="75" x2="250" y2="75" stroke="#1e293b" stroke-width="1.5"
        marker-start="url(#ar-auto)" marker-end="url(#ar-auto)"/>
  <text x="150" y="95" text-anchor="middle" font-size="7" fill="#ef4444">
    el start apunta hacia adentro (incorrecto)
  </text>

  <!-- BIEN: orient="auto-start-reverse" → el start se voltea -->
  <text x="150" y="135" text-anchor="middle" font-size="8" font-weight="bold" fill="#166534">
    orient="auto-start-reverse"
  </text>
  <line x1="50" y1="165" x2="250" y2="165" stroke="#1e293b" stroke-width="1.5"
        marker-start="url(#ar-rev)" marker-end="url(#ar-rev)"/>
  <text x="150" y="185" text-anchor="middle" font-size="7" fill="#166534">
    bidireccional con un solo marker
  </text>

  <text x="150" y="210" text-anchor="middle" font-size="6" fill="#94a3b8">
    un solo marker, dos usos: ahorra una definición
  </text>
</svg>
```

---

## `marker-mid` en paths: solo en vértices explícitos

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">marker-mid aparece en los puntos del path</text>

  <defs>
    <marker id="mid-dot" viewBox="0 0 10 10" refX="5" refY="5"
            markerWidth="5" markerHeight="5">
      <circle cx="5" cy="5" r="4" fill="#f59e0b" stroke="#92400e" stroke-width="1"/>
    </marker>
  </defs>

  <!-- Polyline: vértices claramente delimitados -->
  <text x="150" y="42" text-anchor="middle" font-size="8" font-weight="bold" fill="#1e293b">
    polyline: marker en cada vértice
  </text>
  <polyline points="30,65 75,85 120,55 165,90 210,60 255,80"
            fill="none" stroke="#64748b" stroke-width="1.5"
            marker="url(#mid-dot)"/>

  <!-- Path con curvas: los markers solo en los puntos, no en la curva -->
  <text x="150" y="125" text-anchor="middle" font-size="8" font-weight="bold" fill="#1e293b">
    path con curvas: solo en los anclas
  </text>
  <path d="M 30 155 Q 90 130 150 155 T 270 155"
        fill="none" stroke="#64748b" stroke-width="1.5"
        marker="url(#mid-dot)"/>

  <text x="150" y="195" text-anchor="middle" font-size="7" fill="#94a3b8">
    en el path Q/T solo hay 3 puntos explícitos:
  </text>
  <text x="150" y="208" text-anchor="middle" font-size="7" fill="#94a3b8">
    inicio, fin de la Q y fin de la T — no en medio de la curva
  </text>
</svg>
```

---

## El marker se aplica también a `<path>` complejos

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">markers en paths con curvas</text>

  <defs>
    <marker id="pt-arrow" viewBox="0 0 10 10" refX="9" refY="5"
            markerWidth="6" markerHeight="6" orient="auto">
      <path d="M 0 0 L 10 5 L 0 10 z" fill="#3b82f6"/>
    </marker>
    <marker id="pt-dot" viewBox="0 0 10 10" refX="5" refY="5"
            markerWidth="5" markerHeight="5">
      <circle cx="5" cy="5" r="4" fill="#8b5cf6"/>
    </marker>
  </defs>

  <!-- Curva con flecha al final -->
  <path d="M 30 75 Q 150 30 270 75"
        fill="none" stroke="#3b82f6" stroke-width="1.5"
        marker-end="url(#pt-arrow)"/>
  <text x="30" y="100" font-size="7" fill="#64748b">curva cuadrática · marker-end</text>

  <!-- S-curve con flecha al final -->
  <path d="M 30 140 C 90 100, 210 180, 270 140"
        fill="none" stroke="#3b82f6" stroke-width="1.5"
        marker-end="url(#pt-arrow)"/>
  <text x="30" y="165" font-size="7" fill="#64748b">curva cúbica · marker-end</text>

  <!-- Path con varios subpaths: marker-start/end en cada M -->
  <path d="M 30 190 L 90 190 M 120 190 L 180 190 M 210 190 L 270 190"
        fill="none" stroke="#8b5cf6" stroke-width="1.5"
        marker-start="url(#pt-dot)"
        marker-end="url(#pt-arrow)"/>
  <text x="30" y="210" font-size="6" fill="#94a3b8">múltiples subpaths (M): cada uno tiene su start/end</text>
</svg>
```
