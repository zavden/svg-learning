# 20.3 Animación de movimiento `<animateMotion>`

`<animateMotion>` mueve un elemento **a lo largo de un trazado**. En vez de animar `cx`/`cy` o `transform: translate` por separado, defines un path como "carril" y el elemento lo recorre. Es especialmente potente para curvas, círculos, paths complejos.

---

## Sintaxis básica

```svg
<svg width="300" height="200"
     viewBox="0 0 300 200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    &lt;animateMotion&gt; con atributo path
  </text>

  <rect x="15" y="38" width="270" height="150" fill="#0f172a" rx="3"/>
  <text x="25" y="56" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;circle r="6" fill="steelblue"&gt;</text>
  <text x="25" y="72" font-size="8" font-family="monospace" fill="#60a5fa">  &lt;animateMotion</text>
  <text x="25" y="86" font-size="8" font-family="monospace" fill="#fbbf24">    path="M 10 50 Q 100 10 190 50 T 280 50"</text>
  <text x="25" y="100" font-size="8" font-family="monospace" fill="#34d399">    dur="3s"</text>
  <text x="25" y="114" font-size="8" font-family="monospace" fill="#ec4899">    repeatCount="indefinite"</text>
  <text x="25" y="128" font-size="8" font-family="monospace" fill="#60a5fa">  /&gt;</text>
  <text x="25" y="142" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;/circle&gt;</text>
  <text x="25" y="166" font-size="7" fill="#94a3b8">/* el círculo se mueve por la curva durante 3s */</text>
  <text x="25" y="180" font-size="7" fill="#94a3b8">/* el path NO se renderiza, solo es el carril */</text>
</svg>
```

---

## Demo en vivo

```svg
<svg width="300" height="200"
     viewBox="0 0 300 200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    movimiento sobre curva
  </text>

  <!-- Carril visible -->
  <path d="M 30 110 Q 90 40 150 110 T 270 110" fill="none" stroke="#cbd5e1" stroke-width="1.5" stroke-dasharray="3 3"/>

  <!-- Bola animada -->
  <circle r="8" fill="#3b82f6">
    <animateMotion path="M 30 110 Q 90 40 150 110 T 270 110" dur="3s" repeatCount="indefinite"/>
  </circle>

  <text x="150" y="170" text-anchor="middle" font-size="8" fill="#94a3b8">
    el path gris es solo de referencia visual
  </text>
  <text x="150" y="184" text-anchor="middle" font-size="7" fill="#94a3b8">
    el animateMotion lleva un duplicado del path como carril
  </text>
</svg>
```

---

## `rotate="auto"` para alinear con la curva

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    rotate="auto" — orientación tangente
  </text>

  <!-- Carril -->
  <path d="M 30 120 Q 150 30 270 120" fill="none" stroke="#cbd5e1" stroke-width="1.5" stroke-dasharray="3 3"/>

  <!-- Avión que sigue tangente -->
  <g>
    <path d="M -8 -3 L 8 0 L -8 3 Z" fill="#3b82f6"/>
    <animateMotion path="M 30 120 Q 150 30 270 120" dur="3s" repeatCount="indefinite" rotate="auto"/>
  </g>

  <rect x="15" y="150" width="270" height="62" fill="#0f172a" rx="3"/>
  <text x="25" y="166" font-size="8" font-family="monospace" fill="#94a3b8">/* valores de rotate */</text>
  <text x="25" y="180" font-size="8" font-family="monospace" fill="#34d399">rotate="auto"          /* sigue tangente */</text>
  <text x="25" y="192" font-size="8" font-family="monospace" fill="#34d399">rotate="auto-reverse"  /* tangente + 180° */</text>
  <text x="25" y="204" font-size="8" font-family="monospace" fill="#34d399">rotate="45"            /* ángulo fijo */</text>
</svg>
```

---

## Sub-elemento `<mpath>`: reutilizar un path existente

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    &lt;mpath href="#otroPath"/&gt;
  </text>

  <rect x="15" y="38" width="270" height="160" fill="#0f172a" rx="3"/>
  <text x="25" y="56" font-size="7" font-family="monospace" fill="#94a3b8">/* el path se renderiza visualmente */</text>
  <text x="25" y="72" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;path id="ruta"</text>
  <text x="25" y="86" font-size="8" font-family="monospace" fill="#fbbf24">  d="M 10 50 Q 100 10 190 50"</text>
  <text x="25" y="100" font-size="8" font-family="monospace" fill="#fbbf24">  fill="none" stroke="#888"/&gt;</text>

  <text x="25" y="120" font-size="7" font-family="monospace" fill="#94a3b8">/* y la animación reutiliza ese path */</text>
  <text x="25" y="134" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;circle r="6" fill="red"&gt;</text>
  <text x="25" y="148" font-size="8" font-family="monospace" fill="#60a5fa">  &lt;animateMotion dur="3s" repeatCount="indefinite"&gt;</text>
  <text x="25" y="162" font-size="8" font-family="monospace" fill="#34d399">    &lt;mpath href="#ruta"/&gt;</text>
  <text x="25" y="176" font-size="8" font-family="monospace" fill="#60a5fa">  &lt;/animateMotion&gt;</text>
  <text x="25" y="190" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;/circle&gt;</text>

  <rect x="15" y="208" width="270" height="60" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="224" font-size="9" font-weight="bold" fill="#166534">Ventaja</text>
  <text x="25" y="240" font-size="7" fill="#475569">• un solo path para visualizar Y animar</text>
  <text x="25" y="252" font-size="7" fill="#475569">• si cambias el path, la animación se actualiza sola</text>
  <text x="25" y="264" font-size="7" fill="#475569">• evita duplicar el atributo d en dos sitios</text>
</svg>
```

---

## Cuándo usar `<animateMotion>` vs animar `cx`/`cy`

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    elegir &lt;animateMotion&gt; o &lt;animate&gt;
  </text>

  <rect x="15" y="38" width="270" height="86" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#166534">animateMotion mejor para…</text>
  <text x="25" y="70" font-size="7" fill="#475569">• curvas, círculos, paths complejos</text>
  <text x="25" y="84" font-size="7" fill="#475569">• elementos que deben "seguir un camino"</text>
  <text x="25" y="98" font-size="7" fill="#475569">• animaciones donde la orientación importa (rotate=auto)</text>
  <text x="25" y="112" font-size="7" fill="#475569">• cuando quieres reutilizar un path existente</text>

  <rect x="15" y="132" width="270" height="86" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="148" font-size="9" font-weight="bold" fill="#1e40af">animate cx/cy mejor para…</text>
  <text x="25" y="164" font-size="7" fill="#475569">• movimientos rectilíneos sencillos</text>
  <text x="25" y="178" font-size="7" fill="#475569">• un eje cada vez (oscilación, rebote)</text>
  <text x="25" y="192" font-size="7" fill="#475569">• cuando ya tienes el valor exacto en values</text>
  <text x="25" y="206" font-size="7" fill="#475569">• animaciones que deben sincronizarse con otros atributos</text>
</svg>
```
