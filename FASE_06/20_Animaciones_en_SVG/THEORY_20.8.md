# 20.8 Técnicas de animación comunes

Hay un puñado de patrones de animación SVG que aparecen en casi todo proyecto: dibujar líneas, hacer morphing, spinners, barras de progreso. Conviene tenerlos como recetas listas porque combinan SMIL/CSS/JS de forma característica.

---

## Line drawing — el efecto "se dibuja"

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    stroke-dasharray + stroke-dashoffset
  </text>

  <rect x="15" y="38" width="270" height="84" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="8" font-weight="bold" fill="#94a3b8">El truco</text>
  <text x="25" y="70" font-size="7" fill="#e2e8f0">1) dasharray = longitud total → un solo "trazo"</text>
  <text x="25" y="84" font-size="7" fill="#e2e8f0">2) dashoffset = longitud → el trazo está fuera (invisible)</text>
  <text x="25" y="98" font-size="7" fill="#e2e8f0">3) animar dashoffset → 0 → el trazo "entra" y se ve dibujándose</text>
  <text x="25" y="114" font-size="7" fill="#fbbf24">→ se anima a lo largo del path automáticamente</text>

  <rect x="15" y="132" width="270" height="138" fill="#0f172a" rx="3"/>
  <text x="25" y="148" font-size="7" font-family="monospace" fill="#94a3b8">/* CSS */</text>
  <text x="25" y="162" font-size="8" font-family="monospace" fill="#60a5fa">.dibujar {</text>
  <text x="25" y="176" font-size="8" font-family="monospace" fill="#34d399">  stroke-dasharray: 500;   /* longitud */</text>
  <text x="25" y="190" font-size="8" font-family="monospace" fill="#34d399">  stroke-dashoffset: 500;</text>
  <text x="25" y="204" font-size="8" font-family="monospace" fill="#34d399">  animation: dibujar 2s ease forwards;</text>
  <text x="25" y="218" font-size="8" font-family="monospace" fill="#60a5fa">}</text>
  <text x="25" y="232" font-size="8" font-family="monospace" fill="#fbbf24">@keyframes dibujar {</text>
  <text x="25" y="246" font-size="8" font-family="monospace" fill="#ec4899">  to { stroke-dashoffset: 0; }</text>
  <text x="25" y="260" font-size="8" font-family="monospace" fill="#fbbf24">}</text>
</svg>
```

---

## Demo de line drawing

```svg
<svg width="300" height="180"
     viewBox="0 0 300 180"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    el path se "dibuja" en bucle
  </text>

  <path d="M 30 100 Q 100 30 150 100 T 270 100"
        fill="none" stroke="#3b82f6" stroke-width="3"
        stroke-dasharray="300" stroke-dashoffset="300">
    <animate attributeName="stroke-dashoffset" from="300" to="0"
             dur="2s" repeatCount="indefinite"/>
  </path>

  <text x="150" y="160" text-anchor="middle" font-size="8" fill="#94a3b8">
    300 es la longitud aproximada del path
  </text>
  <text x="150" y="172" text-anchor="middle" font-size="7" fill="#94a3b8">
    para exactitud usa pathElement.getTotalLength() en JS
  </text>
</svg>
```

---

## Morphing de paths

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    animar el atributo d
  </text>

  <rect x="15" y="38" width="270" height="84" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#92400e">Requisito crítico</text>
  <text x="25" y="70" font-size="7" fill="#475569">los dos paths deben tener EL MISMO número</text>
  <text x="25" y="84" font-size="7" fill="#475569">de comandos y puntos para que el morph funcione</text>
  <text x="25" y="98" font-size="7" fill="#475569">si no coincide: animación errática o no aparece</text>
  <text x="25" y="112" font-size="7" fill="#92400e">→ trampa: a veces hay que añadir puntos "fantasma"</text>

  <rect x="15" y="130" width="270" height="138" fill="#0f172a" rx="3"/>
  <text x="25" y="146" font-size="7" font-family="monospace" fill="#94a3b8">/* SMIL — la forma más sencilla */</text>
  <text x="25" y="162" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;path d="M 10 80 Q 50 10 90 80"&gt;</text>
  <text x="25" y="178" font-size="8" font-family="monospace" fill="#60a5fa">  &lt;animate</text>
  <text x="25" y="194" font-size="8" font-family="monospace" fill="#fbbf24">    attributeName="d"</text>
  <text x="25" y="210" font-size="8" font-family="monospace" fill="#34d399">    values="M 10 80 Q 50 10 90 80;</text>
  <text x="25" y="222" font-size="8" font-family="monospace" fill="#34d399">            M 10 80 Q 50 150 90 80;</text>
  <text x="25" y="234" font-size="8" font-family="monospace" fill="#34d399">            M 10 80 Q 50 10 90 80"</text>
  <text x="25" y="248" font-size="8" font-family="monospace" fill="#34d399">    dur="2s" repeatCount="indefinite"/&gt;</text>
  <text x="25" y="262" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;/path&gt;</text>
</svg>
```

---

## Spinner / loader

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    spinner — arco rotando
  </text>

  <!-- Spinner real -->
  <g transform="translate(150 110)">
    <circle r="20" fill="none" stroke="#e2e8f0" stroke-width="4"/>
    <circle r="20" fill="none" stroke="#3b82f6" stroke-width="4"
            stroke-dasharray="90 50" stroke-linecap="round">
      <animateTransform attributeName="transform" type="rotate"
        from="0" to="360" dur="1s" repeatCount="indefinite"/>
    </circle>
  </g>

  <rect x="15" y="158" width="270" height="50" fill="#0f172a" rx="3"/>
  <text x="25" y="174" font-size="7" font-family="monospace" fill="#94a3b8">/* receta */</text>
  <text x="25" y="186" font-size="7" font-family="monospace" fill="#34d399">stroke-dasharray="circunferencia/2 circunferencia/2"</text>
  <text x="25" y="198" font-size="7" font-family="monospace" fill="#34d399">animateTransform rotate 0→360 con repeatCount infinito</text>
</svg>
```

---

## Progress bar circular

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    barra circular con stroke-dashoffset
  </text>

  <!-- Visual demo -->
  <g transform="translate(150 100)">
    <circle r="35" fill="none" stroke="#e2e8f0" stroke-width="6"/>
    <circle r="35" fill="none" stroke="#10b981" stroke-width="6"
            stroke-dasharray="220" stroke-dashoffset="55"
            stroke-linecap="round"
            transform="rotate(-90)"/>
    <text y="5" text-anchor="middle" font-size="14" font-weight="bold" fill="#1e293b">75%</text>
  </g>

  <rect x="15" y="160" width="270" height="108" fill="#0f172a" rx="3"/>
  <text x="25" y="176" font-size="7" font-family="monospace" fill="#94a3b8">/* fórmula */</text>
  <text x="25" y="192" font-size="7" font-family="monospace" fill="#34d399">circunferencia = 2 × π × r</text>
  <text x="25" y="208" font-size="7" font-family="monospace" fill="#34d399">offset = circunferencia × (1 - porcentaje/100)</text>
  <text x="25" y="226" font-size="7" font-family="monospace" fill="#94a3b8">/* en este ejemplo: r=35 */</text>
  <text x="25" y="240" font-size="7" font-family="monospace" fill="#fbbf24">circ = 2 × 3.1416 × 35 ≈ 220</text>
  <text x="25" y="252" font-size="7" font-family="monospace" fill="#fbbf24">75% → offset = 220 × 0.25 ≈ 55</text>
  <text x="25" y="264" font-size="7" font-family="monospace" fill="#94a3b8">rotate(-90) → arrancar arriba, no a la derecha</text>
</svg>
```
