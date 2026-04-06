# 20.4 Animación de transformaciones `<animateTransform>`

`<animateTransform>` es la versión especializada de `<animate>` para el atributo `transform`. Existe porque `<animate>` no entiende la sintaxis compuesta de `translate(...) rotate(...)` y produciría resultados erróneos. `<animateTransform>` sí la entiende y permite combinar transformaciones con `additive="sum"`.

---

## Sintaxis y atributo `type`

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    type indica qué transformación
  </text>

  <rect x="15" y="38" width="270" height="190" fill="#0f172a" rx="3"/>
  <text x="25" y="56" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;rect width="40" height="40"&gt;</text>
  <text x="25" y="72" font-size="8" font-family="monospace" fill="#60a5fa">  &lt;animateTransform</text>
  <text x="25" y="86" font-size="8" font-family="monospace" fill="#fbbf24">    attributeName="transform"</text>
  <text x="25" y="100" font-size="8" font-family="monospace" fill="#fbbf24">    type="rotate"</text>
  <text x="25" y="114" font-size="8" font-family="monospace" fill="#34d399">    from="0 50 50"</text>
  <text x="25" y="128" font-size="8" font-family="monospace" fill="#34d399">    to="360 50 50"</text>
  <text x="25" y="142" font-size="8" font-family="monospace" fill="#34d399">    dur="3s"</text>
  <text x="25" y="156" font-size="8" font-family="monospace" fill="#ec4899">    repeatCount="indefinite"/&gt;</text>
  <text x="25" y="170" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;/rect&gt;</text>

  <text x="25" y="194" font-size="7" fill="#94a3b8">/* type acepta: */</text>
  <text x="25" y="208" font-size="7" font-family="monospace" fill="#fbbf24">translate · rotate · scale · skewX · skewY</text>
  <text x="25" y="220" font-size="7" fill="#94a3b8">los valores se escriben con la sintaxis del propio transform</text>
</svg>
```

---

## Demo: rotación

```svg
<svg width="300" height="200"
     viewBox="0 0 300 200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    rotación SMIL — type="rotate"
  </text>

  <g transform="translate(150 110)">
    <rect x="-25" y="-25" width="50" height="50" fill="#3b82f6">
      <animateTransform attributeName="transform" type="rotate"
        from="0" to="360" dur="3s" repeatCount="indefinite"/>
    </rect>
  </g>

  <text x="150" y="190" text-anchor="middle" font-size="7" fill="#94a3b8">
    el rect rota desde 0° hasta 360° en 3s, infinito
  </text>
</svg>
```

---

## El truco del centro de rotación

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    rotate acepta tres números: ángulo + cx cy
  </text>

  <rect x="15" y="38" width="270" height="80" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#991b1b">✗ rota desde el origen (0 0)</text>
  <text x="25" y="68" font-size="7" font-family="monospace" fill="#475569">&lt;animateTransform type="rotate"</text>
  <text x="25" y="80" font-size="7" font-family="monospace" fill="#475569">  from="0" to="360" .../&gt;</text>
  <text x="25" y="94" font-size="7" fill="#991b1b">el elemento se va volando lejos del centro</text>

  <rect x="15" y="126" width="270" height="100" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="142" font-size="9" font-weight="bold" fill="#166534">✓ especifica el centro de rotación</text>
  <text x="25" y="158" font-size="7" font-family="monospace" fill="#475569">&lt;animateTransform type="rotate"</text>
  <text x="25" y="170" font-size="7" font-family="monospace" fill="#475569">  from="0   50 50"</text>
  <text x="25" y="182" font-size="7" font-family="monospace" fill="#475569">  to  ="360 50 50" .../&gt;</text>
  <text x="25" y="196" font-size="7" fill="#475569">los tres números: ángulo, cx, cy</text>
  <text x="25" y="210" font-size="7" fill="#166534">→ alternativa: envolver en &lt;g&gt; y rotar 0→360 sin cx,cy</text>
</svg>
```

---

## Componer transformaciones con `additive="sum"`

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    rotar Y trasladar al mismo tiempo
  </text>

  <rect x="15" y="38" width="270" height="100" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#991b1b">✗ sin additive — solo gana la última</text>
  <text x="25" y="68" font-size="7" font-family="monospace" fill="#475569">&lt;animateTransform type="rotate" .../&gt;</text>
  <text x="25" y="80" font-size="7" font-family="monospace" fill="#475569">&lt;animateTransform type="translate" .../&gt;</text>
  <text x="25" y="94" font-size="7" fill="#991b1b">solo se ve la traslación, la rotación se pierde</text>
  <text x="25" y="108" font-size="7" fill="#991b1b">defecto: additive="replace" → reemplaza</text>

  <rect x="15" y="146" width="270" height="120" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="162" font-size="9" font-weight="bold" fill="#166534">✓ con additive="sum" — se combinan</text>
  <text x="25" y="178" font-size="7" font-family="monospace" fill="#475569">&lt;animateTransform type="rotate"</text>
  <text x="25" y="190" font-size="7" font-family="monospace" fill="#475569">  from="0" to="360" dur="3s"</text>
  <text x="25" y="202" font-size="7" font-family="monospace" fill="#475569">  additive="sum"</text>
  <text x="25" y="214" font-size="7" font-family="monospace" fill="#475569">  repeatCount="indefinite"/&gt;</text>
  <text x="25" y="228" font-size="7" font-family="monospace" fill="#475569">&lt;animateTransform type="translate"</text>
  <text x="25" y="240" font-size="7" font-family="monospace" fill="#475569">  from="0 0" to="50 0" dur="2s"</text>
  <text x="25" y="252" font-size="7" font-family="monospace" fill="#475569">  additive="sum" .../&gt;</text>
</svg>
```

---

## Ejemplo: planeta orbitando

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    composición: traslación + rotación
  </text>

  <!-- Sol -->
  <circle cx="150" cy="120" r="20" fill="#f59e0b"/>

  <!-- Órbita visible -->
  <circle cx="150" cy="120" r="60" fill="none" stroke="#cbd5e1" stroke-dasharray="2 2"/>

  <!-- Planeta -->
  <g transform="translate(150 120)">
    <g>
      <circle cx="60" cy="0" r="8" fill="#3b82f6"/>
      <animateTransform attributeName="transform" type="rotate"
        from="0" to="360" dur="6s" repeatCount="indefinite"/>
    </g>
  </g>

  <text x="150" y="195" text-anchor="middle" font-size="7" fill="#94a3b8">
    el círculo está a 60px del centro del &lt;g&gt; padre,
  </text>
  <text x="150" y="207" text-anchor="middle" font-size="7" fill="#94a3b8">
    y el &lt;g&gt; rota → órbita perfecta
  </text>
</svg>
```
