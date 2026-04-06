# 19.1 Tres formas de aplicar CSS a SVG

SVG acepta estilos por **tres caminos distintos**, cada uno con sus reglas, su prioridad en la cascada y su rango de propiedades válidas. Confundirlas es la fuente más común de "no me funciona el CSS".

---

## Las tres formas

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    tres formas, una misma propiedad
  </text>

  <!-- Forma 1 -->
  <rect x="15" y="38" width="270" height="70" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#1e40af">1. Atributo de presentación</text>
  <text x="25" y="70" font-size="7" font-family="monospace" fill="#475569">&lt;circle fill="steelblue" stroke="#333"</text>
  <text x="25" y="82" font-size="7" font-family="monospace" fill="#475569">        r="40" cx="50" cy="50" /&gt;</text>
  <text x="25" y="98" font-size="7" fill="#475569">menor prioridad — sobrescribible por CSS</text>

  <!-- Forma 2 -->
  <rect x="15" y="116" width="270" height="62" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="132" font-size="9" font-weight="bold" fill="#166534">2. Estilo inline</text>
  <text x="25" y="148" font-size="7" font-family="monospace" fill="#475569">&lt;circle style="fill: steelblue;</text>
  <text x="25" y="160" font-size="7" font-family="monospace" fill="#475569">        stroke: #333;" r="40" /&gt;</text>
  <text x="25" y="172" font-size="7" fill="#475569">alta prioridad — solo !important lo supera</text>

  <!-- Forma 3 -->
  <rect x="15" y="186" width="270" height="80" fill="#ede9fe" stroke="#8b5cf6" rx="3"/>
  <text x="25" y="202" font-size="9" font-weight="bold" fill="#5b21b6">3. Hoja de estilos (interna o externa)</text>
  <text x="25" y="218" font-size="7" font-family="monospace" fill="#475569">&lt;style&gt;</text>
  <text x="25" y="230" font-size="7" font-family="monospace" fill="#475569">  circle { fill: steelblue; }</text>
  <text x="25" y="242" font-size="7" font-family="monospace" fill="#475569">&lt;/style&gt;</text>
  <text x="25" y="258" font-size="7" fill="#475569">prioridad por especificidad CSS estándar</text>
</svg>
```

---

## Atributos de presentación

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    atributos de presentación
  </text>

  <!-- Demo visual -->
  <circle cx="80" cy="70" r="28" fill="#3b82f6" stroke="#1e293b" stroke-width="3"/>
  <text x="80" y="115" text-anchor="middle" font-size="8" fill="#475569">cx, cy, r</text>
  <text x="80" y="125" text-anchor="middle" font-size="7" fill="#94a3b8">geometría</text>

  <circle cx="160" cy="70" r="28" fill="#10b981" stroke="#1e293b" stroke-width="3"/>
  <text x="160" y="115" text-anchor="middle" font-size="8" fill="#475569">fill, stroke</text>
  <text x="160" y="125" text-anchor="middle" font-size="7" fill="#94a3b8">presentación</text>

  <circle cx="240" cy="70" r="28" fill="#f59e0b" stroke="#1e293b" stroke-width="3" opacity="0.6"/>
  <text x="240" y="115" text-anchor="middle" font-size="8" fill="#475569">opacity</text>
  <text x="240" y="125" text-anchor="middle" font-size="7" fill="#94a3b8">presentación</text>

  <!-- Características -->
  <rect x="15" y="138" width="270" height="92" fill="#0f172a" rx="3"/>
  <text x="25" y="154" font-size="8" font-weight="bold" fill="#94a3b8">Características</text>
  <text x="25" y="170" font-size="7" fill="#e2e8f0">• sintaxis nativa de SVG: la usan los editores</text>
  <text x="25" y="184" font-size="7" fill="#e2e8f0">• menor prioridad en la cascada</text>
  <text x="25" y="198" font-size="7" fill="#e2e8f0">• solo aceptan valores SVG (sin calc, var, etc.)</text>
  <text x="25" y="212" font-size="7" fill="#e2e8f0">• no toda propiedad CSS tiene equivalente</text>
  <text x="25" y="226" font-size="7" fill="#fbbf24">• los más comunes: fill, stroke, opacity, font-*</text>
</svg>
```

---

## Estilo inline (atributo `style`)

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    estilo inline: style="..."
  </text>

  <!-- Código -->
  <rect x="15" y="38" width="270" height="80" fill="#0f172a" rx="3"/>
  <text x="25" y="56" font-size="8" font-family="monospace" fill="#94a3b8">/* aceptan TODA la sintaxis CSS */</text>
  <text x="25" y="72" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;circle style="</text>
  <text x="25" y="86" font-size="8" font-family="monospace" fill="#60a5fa">  fill: var(--color-marca);</text>
  <text x="25" y="100" font-size="8" font-family="monospace" fill="#34d399">  stroke-width: calc(2px * var(--escala));</text>
  <text x="25" y="114" font-size="8" font-family="monospace" fill="#e2e8f0">" /&gt;</text>

  <!-- Comparación -->
  <rect x="15" y="128" width="270" height="100" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="144" font-size="9" font-weight="bold" fill="#166534">Diferencias clave con atributo de presentación</text>
  <text x="25" y="160" font-size="7" fill="#475569">• acepta funciones CSS modernas: var(), calc(), env()</text>
  <text x="25" y="174" font-size="7" fill="#475569">• acepta cualquier propiedad CSS, no solo las "SVG"</text>
  <text x="25" y="188" font-size="7" fill="#475569">• prioridad alta: solo !important lo supera</text>
  <text x="25" y="202" font-size="7" fill="#991b1b">• no se sobrescribe con CSS normal — frágil para temas</text>
  <text x="25" y="216" font-size="7" fill="#475569">• el editor gráfico no lo genera por defecto</text>
</svg>
```

---

## Hoja de estilos: bloque `<style>` interno

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    &lt;style&gt; dentro del SVG
  </text>

  <rect x="15" y="38" width="270" height="138" fill="#0f172a" rx="3"/>
  <text x="25" y="56" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;svg viewBox="0 0 100 100"&gt;</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#60a5fa">  &lt;style&gt;</text>
  <text x="25" y="84" font-size="8" font-family="monospace" fill="#34d399">    circle { fill: steelblue; }</text>
  <text x="25" y="98" font-size="8" font-family="monospace" fill="#34d399">    .destacado { stroke: red; }</text>
  <text x="25" y="112" font-size="8" font-family="monospace" fill="#34d399">    #principal { opacity: 0.8; }</text>
  <text x="25" y="126" font-size="8" font-family="monospace" fill="#60a5fa">  &lt;/style&gt;</text>
  <text x="25" y="140" font-size="8" font-family="monospace" fill="#e2e8f0">  &lt;circle r="40" cx="50" cy="50"</text>
  <text x="25" y="154" font-size="8" font-family="monospace" fill="#fbbf24">          class="destacado" id="principal"/&gt;</text>
  <text x="25" y="168" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;/svg&gt;</text>

  <rect x="15" y="184" width="270" height="64" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="200" font-size="9" font-weight="bold" fill="#166534">Mejor opción cuando…</text>
  <text x="25" y="216" font-size="7" fill="#475569">• hay muchos elementos del mismo tipo o clase</text>
  <text x="25" y="230" font-size="7" fill="#475569">• necesitas variables, animaciones o media queries</text>
  <text x="25" y="244" font-size="7" fill="#475569">• quieres reutilizar estilos sin repetir atributos</text>
</svg>
```

---

## Hoja de estilos externa (solo SVG inline)

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    CSS externo del documento
  </text>

  <rect x="15" y="38" width="270" height="74" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="8" font-family="monospace" fill="#94a3b8">/* archivo styles.css */</text>
  <text x="25" y="68" font-size="8" font-family="monospace" fill="#e2e8f0">svg circle {</text>
  <text x="25" y="82" font-size="8" font-family="monospace" fill="#34d399">  fill: steelblue;</text>
  <text x="25" y="96" font-size="8" font-family="monospace" fill="#e2e8f0">}</text>

  <rect x="15" y="120" width="270" height="92" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="136" font-size="9" font-weight="bold" fill="#92400e">Importante</text>
  <text x="25" y="152" font-size="7" fill="#475569">• funciona solo si el SVG es inline en el HTML</text>
  <text x="25" y="166" font-size="7" fill="#475569">• no aplica si el SVG se carga con &lt;img&gt;</text>
  <text x="25" y="180" font-size="7" fill="#475569">• tampoco con CSS background-image</text>
  <text x="25" y="194" font-size="7" fill="#475569">• &lt;object&gt; y &lt;iframe&gt; tienen su propio scope CSS</text>
  <text x="25" y="206" font-size="7" fill="#92400e">→ ver sección 18 sobre métodos de integración</text>
</svg>
```

---

## Decisión rápida

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    ¿qué forma elijo?
  </text>

  <rect x="10" y="32" width="280" height="20" fill="#e2e8f0"/>
  <text x="20" y="46" font-size="7" font-weight="bold" fill="#1e293b">Caso</text>
  <text x="180" y="46" font-size="7" font-weight="bold" fill="#1e293b">Forma</text>

  <rect x="10" y="52" width="280" height="20" fill="white"/>
  <text x="20" y="65" font-size="7" fill="#475569">SVG generado por editor (Figma, etc.)</text>
  <text x="180" y="65" font-size="7" font-family="monospace" fill="#3b82f6">atributos</text>

  <rect x="10" y="72" width="280" height="20" fill="#f8fafc"/>
  <text x="20" y="85" font-size="7" fill="#475569">Estilos sobrescribibles desde fuera</text>
  <text x="180" y="85" font-size="7" font-family="monospace" fill="#3b82f6">atributos</text>

  <rect x="10" y="92" width="280" height="20" fill="white"/>
  <text x="20" y="105" font-size="7" fill="#475569">Muchos elementos misma clase</text>
  <text x="180" y="105" font-size="7" font-family="monospace" fill="#10b981">&lt;style&gt; interno</text>

  <rect x="10" y="112" width="280" height="20" fill="#f8fafc"/>
  <text x="20" y="125" font-size="7" fill="#475569">Animación o media query</text>
  <text x="180" y="125" font-size="7" font-family="monospace" fill="#10b981">&lt;style&gt; interno</text>

  <rect x="10" y="132" width="280" height="20" fill="white"/>
  <text x="20" y="145" font-size="7" fill="#475569">Variables del documento padre</text>
  <text x="180" y="145" font-size="7" font-family="monospace" fill="#10b981">CSS externo</text>

  <rect x="10" y="152" width="280" height="20" fill="#f8fafc"/>
  <text x="20" y="165" font-size="7" fill="#475569">Estilo no debe sobrescribirse</text>
  <text x="180" y="165" font-size="7" font-family="monospace" fill="#8b5cf6">style="..." inline</text>

  <rect x="10" y="172" width="280" height="20" fill="white"/>
  <text x="20" y="185" font-size="7" fill="#475569">Valor con var() o calc()</text>
  <text x="180" y="185" font-size="7" font-family="monospace" fill="#8b5cf6">style="..." inline</text>

  <text x="150" y="208" text-anchor="middle" font-size="7" fill="#94a3b8">en proyectos reales se mezclan los tres a la vez</text>
</svg>
```
