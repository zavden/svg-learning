# Referencia rápida — SVG responsivo y accesibilidad

---

## Cheat sheet: SVG responsivo mínimo

```svg
<svg width="300" height="200"
     viewBox="0 0 300 200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    receta copy-paste
  </text>

  <!-- HTML -->
  <rect x="15" y="38" width="270" height="68" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">&lt;!-- HTML --&gt;</text>
  <text x="25" y="68" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;div class="svg-wrap"&gt;</text>
  <text x="25" y="80" font-size="8" font-family="monospace" fill="#60a5fa">  &lt;svg viewBox="0 0 400 300"&gt;</text>
  <text x="25" y="92" font-size="8" font-family="monospace" fill="#60a5fa">    ...</text>
  <text x="25" y="100" font-size="8" font-family="monospace" fill="#60a5fa">  &lt;/svg&gt;</text>

  <!-- CSS -->
  <rect x="15" y="114" width="270" height="78" fill="#0f172a" rx="3"/>
  <text x="25" y="130" font-size="7" font-family="monospace" fill="#94a3b8">/* CSS */</text>
  <text x="25" y="144" font-size="8" font-family="monospace" fill="#e2e8f0">.svg-wrap {</text>
  <text x="25" y="156" font-size="8" font-family="monospace" fill="#34d399">  width: 100%; max-width: 600px;</text>
  <text x="25" y="168" font-size="8" font-family="monospace" fill="#e2e8f0">}</text>
  <text x="25" y="180" font-size="8" font-family="monospace" fill="#e2e8f0">.svg-wrap svg {</text>
  <text x="25" y="192" font-size="8" font-family="monospace" fill="#fbbf24">  width:100%; height:auto; display:block;</text>
</svg>
```

---

## Cheat sheet: SVG accesible mínimo

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    accesibilidad por caso
  </text>

  <!-- Caso 1: imagen informativa -->
  <rect x="15" y="38" width="270" height="58" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="54" font-size="8" font-weight="bold" fill="#1e40af">Imagen con significado</text>
  <text x="25" y="68" font-size="7" font-family="monospace" fill="#475569">&lt;svg role="img" aria-labelledby="t d"&gt;</text>
  <text x="25" y="80" font-size="7" font-family="monospace" fill="#475569">  &lt;title id="t"&gt;Título corto&lt;/title&gt;</text>
  <text x="25" y="92" font-size="7" font-family="monospace" fill="#475569">  &lt;desc id="d"&gt;Descripción&lt;/desc&gt;</text>

  <!-- Caso 2: decorativa -->
  <rect x="15" y="104" width="270" height="44" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="120" font-size="8" font-weight="bold" fill="#166534">Decorativa</text>
  <text x="25" y="134" font-size="7" font-family="monospace" fill="#475569">&lt;svg aria-hidden="true" focusable="false"&gt;</text>
  <text x="25" y="144" font-size="6" fill="#475569">(sin título, ocultado completamente del AT)</text>

  <!-- Caso 3: ícono en botón con texto -->
  <rect x="15" y="156" width="270" height="44" fill="#ede9fe" stroke="#8b5cf6" rx="3"/>
  <text x="25" y="172" font-size="8" font-weight="bold" fill="#5b21b6">Ícono en botón con texto</text>
  <text x="25" y="186" font-size="7" font-family="monospace" fill="#475569">&lt;button&gt;&lt;svg aria-hidden="true"&gt;.&lt;/svg&gt; Enviar&lt;/button&gt;</text>
  <text x="25" y="196" font-size="6" fill="#475569">(el texto del botón es el nombre accesible)</text>

  <!-- Caso 4: botón solo ícono -->
  <rect x="15" y="208" width="270" height="24" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="22" y="222" font-size="7" font-family="monospace" fill="#475569">&lt;button aria-label="Cerrar"&gt;&lt;svg aria-hidden="true".../&gt;&lt;/button&gt;</text>
</svg>
```

---

## Tabla: qué herramienta para qué caso

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">tabla de decisión</text>

  <!-- Header -->
  <rect x="10" y="28" width="280" height="18" fill="#e2e8f0"/>
  <text x="18" y="40" font-size="7" font-weight="bold" fill="#1e293b">Caso</text>
  <text x="155" y="40" font-size="7" font-weight="bold" fill="#1e293b">Solución</text>

  <!-- Rows -->
  <rect x="10" y="46" width="280" height="22" fill="white"/>
  <text x="18" y="60" font-size="7" fill="#475569">SVG debe llenar su contenedor</text>
  <text x="155" y="60" font-size="7" font-family="monospace" fill="#3b82f6">width:100%; height:auto</text>

  <rect x="10" y="68" width="280" height="22" fill="#f8fafc"/>
  <text x="18" y="82" font-size="7" fill="#475569">Proporción fija sin hacks</text>
  <text x="155" y="82" font-size="7" font-family="monospace" fill="#3b82f6">aspect-ratio: 16/9</text>

  <rect x="10" y="90" width="280" height="22" fill="white"/>
  <text x="18" y="104" font-size="7" fill="#475569">Evitar espacio fantasma inline</text>
  <text x="155" y="104" font-size="7" font-family="monospace" fill="#3b82f6">display: block</text>

  <rect x="10" y="112" width="280" height="22" fill="#f8fafc"/>
  <text x="18" y="126" font-size="7" fill="#475569">Ícono que hereda color</text>
  <text x="155" y="126" font-size="7" font-family="monospace" fill="#10b981">fill="currentColor"</text>

  <rect x="10" y="134" width="280" height="22" fill="white"/>
  <text x="18" y="148" font-size="7" fill="#475569">Modo oscuro automático</text>
  <text x="155" y="148" font-size="7" font-family="monospace" fill="#10b981">prefers-color-scheme</text>

  <rect x="10" y="156" width="280" height="22" fill="#f8fafc"/>
  <text x="18" y="170" font-size="7" fill="#475569">Simplificar en pantallas pequeñas</text>
  <text x="155" y="170" font-size="7" font-family="monospace" fill="#10b981">@media dentro del &lt;svg&gt;</text>

  <rect x="10" y="178" width="280" height="22" fill="white"/>
  <text x="18" y="192" font-size="7" fill="#475569">Imagen con significado</text>
  <text x="155" y="192" font-size="7" font-family="monospace" fill="#8b5cf6">role="img" + &lt;title&gt;</text>

  <rect x="10" y="200" width="280" height="22" fill="#f8fafc"/>
  <text x="18" y="214" font-size="7" fill="#475569">SVG puramente decorativo</text>
  <text x="155" y="214" font-size="7" font-family="monospace" fill="#8b5cf6">aria-hidden="true"</text>

  <rect x="10" y="222" width="280" height="22" fill="white"/>
  <text x="18" y="236" font-size="7" fill="#475569">Botón solo con ícono</text>
  <text x="155" y="236" font-size="7" font-family="monospace" fill="#8b5cf6">aria-label en &lt;button&gt;</text>

  <rect x="10" y="244" width="280" height="22" fill="#f8fafc"/>
  <text x="18" y="258" font-size="7" fill="#475569">Gráfico complejo con datos</text>
  <text x="155" y="258" font-size="7" font-family="monospace" fill="#8b5cf6">tabla .sr-only al lado</text>
</svg>
```

---

## Snippets copy-paste

### SVG responsivo mínimo

```svg
<svg viewBox="0 0 400 300" xmlns="http://www.w3.org/2000/svg">
  <!-- contenido -->
</svg>
```

```css
svg { width: 100%; height: auto; display: block; }
```

### SVG accesible informativo

```svg
<svg role="img" aria-labelledby="t d" viewBox="0 0 200 100">
  <title id="t">Ventas por trimestre</title>
  <desc id="d">T1: 45, T2: 62, T3: 78, T4: 91. Tendencia ascendente.</desc>
  <!-- contenido visual -->
</svg>
```

### SVG decorativo

```svg
<svg aria-hidden="true" focusable="false" viewBox="0 0 24 24">
  <path d="..." fill="currentColor"/>
</svg>
```

### Botón con ícono y texto

```html
<button>
  <svg aria-hidden="true" focusable="false" viewBox="0 0 24 24">
    <path d="..."/>
  </svg>
  Enviar
</button>
```

### Botón solo con ícono

```html
<button aria-label="Cerrar diálogo">
  <svg aria-hidden="true" focusable="false" viewBox="0 0 24 24">
    <path d="..."/>
  </svg>
</button>
```

### Modo oscuro dentro del SVG

```svg
<svg viewBox="0 0 100 100">
  <style>
    :root { --bg: white; --fg: #1a1a2e; }
    @media (prefers-color-scheme: dark) {
      :root { --bg: #1a1a2e; --fg: white; }
    }
    circle { fill: var(--bg); stroke: var(--fg); }
  </style>
  <circle cx="50" cy="50" r="40" stroke-width="2"/>
</svg>
```
