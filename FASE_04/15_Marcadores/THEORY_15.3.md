# 15.3 Usos comunes

Los marcadores son la solución nativa de SVG para flechas, puntos en gráficos y decoraciones de líneas. Esta sección recorre los escenarios más frecuentes.

---

## Cabezas de flecha: estilos y variantes

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">galería de flechas</text>

  <defs>
    <!-- Triángulo sólido clásico -->
    <marker id="a-solid" viewBox="0 0 10 10" refX="9" refY="5"
            markerWidth="7" markerHeight="7" orient="auto">
      <path d="M 0 0 L 10 5 L 0 10 z" fill="#1e293b"/>
    </marker>
    <!-- Triángulo hueco -->
    <marker id="a-hollow" viewBox="0 0 10 10" refX="9" refY="5"
            markerWidth="8" markerHeight="8" orient="auto">
      <path d="M 0 0 L 10 5 L 0 10 z" fill="white" stroke="#1e293b" stroke-width="1.5"/>
    </marker>
    <!-- Punta abierta (V) -->
    <marker id="a-open" viewBox="0 0 10 10" refX="9" refY="5"
            markerWidth="8" markerHeight="8" orient="auto">
      <path d="M 0 1 L 9 5 L 0 9" fill="none" stroke="#1e293b" stroke-width="1.5"/>
    </marker>
    <!-- Punta larga y estrecha -->
    <marker id="a-thin" viewBox="0 0 20 10" refX="18" refY="5"
            markerWidth="10" markerHeight="6" orient="auto">
      <path d="M 0 0 L 20 5 L 0 10 L 5 5 z" fill="#1e293b"/>
    </marker>
    <!-- Diamante -->
    <marker id="a-diamond" viewBox="0 0 10 10" refX="10" refY="5"
            markerWidth="8" markerHeight="6" orient="auto">
      <polygon points="0,5 5,0 10,5 5,10" fill="#1e293b"/>
    </marker>
    <!-- Círculo -->
    <marker id="a-dot" viewBox="0 0 10 10" refX="9" refY="5"
            markerWidth="5" markerHeight="5" orient="auto">
      <circle cx="5" cy="5" r="4" fill="#1e293b"/>
    </marker>
  </defs>

  <line x1="30" y1="45" x2="200" y2="45"   stroke="#1e293b" stroke-width="1.5" marker-end="url(#a-solid)"/>
  <text x="215" y="48" font-size="7" fill="#64748b">sólida</text>

  <line x1="30" y1="75" x2="200" y2="75"   stroke="#1e293b" stroke-width="1.5" marker-end="url(#a-hollow)"/>
  <text x="215" y="78" font-size="7" fill="#64748b">hueca</text>

  <line x1="30" y1="105" x2="200" y2="105" stroke="#1e293b" stroke-width="1.5" marker-end="url(#a-open)"/>
  <text x="215" y="108" font-size="7" fill="#64748b">abierta</text>

  <line x1="30" y1="135" x2="200" y2="135" stroke="#1e293b" stroke-width="1.5" marker-end="url(#a-thin)"/>
  <text x="215" y="138" font-size="7" fill="#64748b">estrecha</text>

  <line x1="30" y1="165" x2="200" y2="165" stroke="#1e293b" stroke-width="1.5" marker-end="url(#a-diamond)"/>
  <text x="215" y="168" font-size="7" fill="#64748b">diamante</text>

  <line x1="30" y1="195" x2="200" y2="195" stroke="#1e293b" stroke-width="1.5" marker-end="url(#a-dot)"/>
  <text x="215" y="198" font-size="7" fill="#64748b">círculo</text>

  <text x="150" y="235" text-anchor="middle" font-size="7" fill="#94a3b8">
    cada estilo: un &lt;marker&gt; con distinto contenido
  </text>
  <text x="150" y="248" text-anchor="middle" font-size="7" fill="#94a3b8">
    refX ajustado para que el "extremo" del símbolo toque el vértice
  </text>
</svg>
```

---

## El problema del trazo que asoma

```svg
<svg width="300" height="200"
     viewBox="0 0 300 200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">cuidado con el trazo bajo la flecha</text>

  <defs>
    <!-- refX=5: el trazo cruza la flecha y asoma -->
    <marker id="over-center" viewBox="0 0 10 10" refX="5" refY="5"
            markerWidth="10" markerHeight="10" orient="auto">
      <path d="M 0 0 L 10 5 L 0 10 z" fill="#ef4444"/>
    </marker>
    <!-- refX=10: el trazo queda tapado por la flecha -->
    <marker id="over-tip" viewBox="0 0 10 10" refX="10" refY="5"
            markerWidth="10" markerHeight="10" orient="auto">
      <path d="M 0 0 L 10 5 L 0 10 z" fill="#10b981"/>
    </marker>
  </defs>

  <rect x="15" y="35" width="130" height="140" fill="#fef2f2" stroke="#fca5a5" rx="4"/>
  <text x="80" y="52" text-anchor="middle" font-size="8" font-weight="bold" fill="#991b1b">MAL</text>
  <text x="80" y="64" text-anchor="middle" font-size="7" fill="#64748b">refX=5</text>
  <line x1="25" y1="105" x2="130" y2="105"
        stroke="#1e293b" stroke-width="4" marker-end="url(#over-center)"/>
  <text x="80" y="155" text-anchor="middle" font-size="6" fill="#ef4444">el trazo negro asoma</text>
  <text x="80" y="165" text-anchor="middle" font-size="6" fill="#ef4444">en el pico de la flecha</text>

  <rect x="155" y="35" width="130" height="140" fill="#dcfce7" stroke="#86efac" rx="4"/>
  <text x="220" y="52" text-anchor="middle" font-size="8" font-weight="bold" fill="#166534">BIEN</text>
  <text x="220" y="64" text-anchor="middle" font-size="7" fill="#64748b">refX=10</text>
  <line x1="165" y1="105" x2="270" y2="105"
        stroke="#1e293b" stroke-width="4" marker-end="url(#over-tip)"/>
  <text x="220" y="155" text-anchor="middle" font-size="6" fill="#166534">la punta del marker</text>
  <text x="220" y="165" text-anchor="middle" font-size="6" fill="#166534">tapa el extremo del trazo</text>
</svg>
```

---

## Gráfico de líneas con puntos en los vértices

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">dots automáticos en un line chart</text>

  <defs>
    <marker id="dp-blue" viewBox="0 0 10 10" refX="5" refY="5"
            markerWidth="6" markerHeight="6" markerUnits="userSpaceOnUse">
      <circle cx="5" cy="5" r="4" fill="white" stroke="#3b82f6" stroke-width="2"/>
    </marker>
    <marker id="dp-pink" viewBox="0 0 10 10" refX="5" refY="5"
            markerWidth="6" markerHeight="6" markerUnits="userSpaceOnUse">
      <rect x="1" y="1" width="8" height="8" fill="white" stroke="#ec4899" stroke-width="2"/>
    </marker>
  </defs>

  <!-- Ejes -->
  <line x1="30" y1="50" x2="30" y2="180" stroke="#cbd5e1" stroke-width="1"/>
  <line x1="30" y1="180" x2="280" y2="180" stroke="#cbd5e1" stroke-width="1"/>

  <!-- Gridlines -->
  <line x1="30" y1="80"  x2="280" y2="80"  stroke="#e2e8f0" stroke-dasharray="2,2"/>
  <line x1="30" y1="120" x2="280" y2="120" stroke="#e2e8f0" stroke-dasharray="2,2"/>
  <line x1="30" y1="150" x2="280" y2="150" stroke="#e2e8f0" stroke-dasharray="2,2"/>

  <!-- Serie 1: azul -->
  <polyline points="45,140 85,100 125,120 165,75 205,90 245,60"
            fill="none" stroke="#3b82f6" stroke-width="2"
            marker="url(#dp-blue)"/>

  <!-- Serie 2: rosa -->
  <polyline points="45,160 85,150 125,135 165,140 205,115 245,120"
            fill="none" stroke="#ec4899" stroke-width="2"
            marker="url(#dp-pink)"/>

  <!-- Leyenda -->
  <rect x="35" y="195" width="6" height="6" fill="white" stroke="#3b82f6" stroke-width="2"/>
  <text x="46" y="201" font-size="6" fill="#64748b">serie A</text>

  <rect x="90" y="195" width="6" height="6" fill="white" stroke="#ec4899" stroke-width="2"/>
  <text x="101" y="201" font-size="6" fill="#64748b">serie B</text>

  <text x="220" y="205" text-anchor="middle" font-size="6" fill="#94a3b8">
    markerUnits="userSpaceOnUse" · tamaño fijo
  </text>
</svg>
```

---

## Diagramas: flechas UML y de relación

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">notaciones UML con markers</text>

  <defs>
    <!-- Herencia: triángulo hueco grande -->
    <marker id="uml-inherit" viewBox="0 0 12 12" refX="11" refY="6"
            markerWidth="10" markerHeight="10" orient="auto">
      <path d="M 0 0 L 11 6 L 0 12 z" fill="white" stroke="#1e293b" stroke-width="1.5"/>
    </marker>
    <!-- Composición: diamante lleno -->
    <marker id="uml-comp" viewBox="0 0 14 10" refX="13" refY="5"
            markerWidth="11" markerHeight="8" orient="auto">
      <polygon points="0,5 7,0 14,5 7,10" fill="#1e293b"/>
    </marker>
    <!-- Agregación: diamante hueco -->
    <marker id="uml-agg" viewBox="0 0 14 10" refX="13" refY="5"
            markerWidth="11" markerHeight="8" orient="auto">
      <polygon points="0,5 7,0 14,5 7,10" fill="white" stroke="#1e293b" stroke-width="1.5"/>
    </marker>
    <!-- Asociación: flecha abierta -->
    <marker id="uml-assoc" viewBox="0 0 10 10" refX="9" refY="5"
            markerWidth="9" markerHeight="9" orient="auto">
      <path d="M 0 0 L 9 5 L 0 10" fill="none" stroke="#1e293b" stroke-width="1.5"/>
    </marker>
  </defs>

  <!-- Herencia -->
  <line x1="100" y1="50" x2="230" y2="50" stroke="#1e293b" stroke-width="1.5" marker-end="url(#uml-inherit)"/>
  <text x="30" y="53" font-size="7" fill="#64748b">herencia</text>

  <!-- Composición -->
  <line x1="100" y1="95" x2="230" y2="95" stroke="#1e293b" stroke-width="1.5" marker-end="url(#uml-comp)"/>
  <text x="30" y="98" font-size="7" fill="#64748b">composición</text>

  <!-- Agregación -->
  <line x1="100" y1="140" x2="230" y2="140" stroke="#1e293b" stroke-width="1.5" marker-end="url(#uml-agg)"/>
  <text x="30" y="143" font-size="7" fill="#64748b">agregación</text>

  <!-- Asociación -->
  <line x1="100" y1="185" x2="230" y2="185" stroke="#1e293b" stroke-width="1.5" marker-end="url(#uml-assoc)"/>
  <text x="30" y="188" font-size="7" fill="#64748b">asociación</text>

  <!-- Dependencia (línea discontinua) -->
  <line x1="100" y1="225" x2="230" y2="225"
        stroke="#1e293b" stroke-width="1.5"
        stroke-dasharray="4,3"
        marker-end="url(#uml-assoc)"/>
  <text x="30" y="228" font-size="7" fill="#64748b">dependencia</text>
</svg>
```

---

## Herencia de color: `context-stroke` y `currentColor`

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">marker que hereda el color del trazo</text>

  <defs>
    <!-- Color FIJO: siempre azul -->
    <marker id="c-fixed" viewBox="0 0 10 10" refX="9" refY="5"
            markerWidth="7" markerHeight="7" orient="auto">
      <path d="M 0 0 L 10 5 L 0 10 z" fill="#3b82f6"/>
    </marker>
    <!-- context-stroke: hereda del stroke del padre (SVG 2) -->
    <marker id="c-ctx" viewBox="0 0 10 10" refX="9" refY="5"
            markerWidth="7" markerHeight="7" orient="auto">
      <path d="M 0 0 L 10 5 L 0 10 z" fill="context-stroke"/>
    </marker>
    <!-- currentColor: hereda la CSS color del padre -->
    <marker id="c-curr" viewBox="0 0 10 10" refX="9" refY="5"
            markerWidth="7" markerHeight="7" orient="auto">
      <path d="M 0 0 L 10 5 L 0 10 z" fill="currentColor"/>
    </marker>
  </defs>

  <!-- MAL: color fijo -->
  <text x="150" y="40" text-anchor="middle" font-size="8" font-weight="bold" fill="#991b1b">
    fill="#3b82f6" (fijo)
  </text>
  <line x1="30" y1="60" x2="140" y2="60" stroke="#ef4444" stroke-width="1.5" marker-end="url(#c-fixed)"/>
  <line x1="30" y1="80" x2="140" y2="80" stroke="#10b981" stroke-width="1.5" marker-end="url(#c-fixed)"/>
  <line x1="30" y1="100" x2="140" y2="100" stroke="#f59e0b" stroke-width="1.5" marker-end="url(#c-fixed)"/>
  <text x="160" y="80" font-size="6" fill="#ef4444">la flecha siempre azul</text>

  <!-- BIEN: context-stroke -->
  <text x="150" y="135" text-anchor="middle" font-size="8" font-weight="bold" fill="#166534">
    fill="context-stroke" (SVG 2)
  </text>
  <line x1="30" y1="155" x2="140" y2="155" stroke="#ef4444" stroke-width="1.5" marker-end="url(#c-ctx)"/>
  <line x1="30" y1="175" x2="140" y2="175" stroke="#10b981" stroke-width="1.5" marker-end="url(#c-ctx)"/>
  <line x1="30" y1="195" x2="140" y2="195" stroke="#f59e0b" stroke-width="1.5" marker-end="url(#c-ctx)"/>
  <text x="160" y="175" font-size="6" fill="#166534">hereda cada color</text>

  <text x="150" y="225" text-anchor="middle" font-size="6" fill="#94a3b8">
    alternativa SVG 1.1: fill="currentColor" + CSS color en el padre
  </text>
</svg>
```

---

## Diagrama de flujo completo

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">un flowchart con markers</text>

  <defs>
    <marker id="flow-arrow" viewBox="0 0 10 10" refX="10" refY="5"
            markerWidth="7" markerHeight="7" orient="auto">
      <path d="M 0 0 L 10 5 L 0 10 z" fill="#475569"/>
    </marker>
  </defs>

  <!-- Start -->
  <ellipse cx="150" cy="45" rx="40" ry="14" fill="#dcfce7" stroke="#10b981" stroke-width="1.5"/>
  <text x="150" y="49" text-anchor="middle" font-size="9" fill="#166534">Start</text>

  <!-- Decision -->
  <polygon points="150,80 205,115 150,150 95,115" fill="#fef9c3" stroke="#f59e0b" stroke-width="1.5"/>
  <text x="150" y="119" text-anchor="middle" font-size="8" fill="#92400e">¿ok?</text>

  <!-- Process A -->
  <rect x="30" y="170" width="80" height="28" rx="4" fill="#dbeafe" stroke="#3b82f6" stroke-width="1.5"/>
  <text x="70" y="189" text-anchor="middle" font-size="8" fill="#1e40af">proceso</text>

  <!-- End -->
  <ellipse cx="230" cy="184" rx="40" ry="14" fill="#fee2e2" stroke="#ef4444" stroke-width="1.5"/>
  <text x="230" y="188" text-anchor="middle" font-size="9" fill="#991b1b">End</text>

  <!-- Flechas -->
  <line x1="150" y1="60" x2="150" y2="75" stroke="#475569" stroke-width="1.5" marker-end="url(#flow-arrow)"/>

  <path d="M 95 115 L 70 115 L 70 165" fill="none" stroke="#475569" stroke-width="1.5" marker-end="url(#flow-arrow)"/>
  <text x="85" y="112" font-size="7" fill="#475569">no</text>

  <path d="M 205 115 L 230 115 L 230 165" fill="none" stroke="#475569" stroke-width="1.5" marker-end="url(#flow-arrow)"/>
  <text x="215" y="112" font-size="7" fill="#475569">sí</text>

  <path d="M 70 198 L 70 235 L 150 235" fill="none" stroke="#475569" stroke-width="1.5" marker-end="url(#flow-arrow)"/>

  <text x="150" y="252" text-anchor="middle" font-size="7" fill="#94a3b8">
    un solo marker reutilizado en todas las conexiones
  </text>
</svg>
```
