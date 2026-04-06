# 17.5 SVG y alto contraste / modo oscuro

Los usuarios pueden **forzar temas alternativos**: modo oscuro del sistema, alto contraste de Windows, preferencias personalizadas. Un SVG robusto se adapta automáticamente a esas preferencias usando CSS moderno.

---

## El problema: colores fijos vs. preferencias del usuario

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    mismo ícono, tres entornos
  </text>

  <!-- Tema claro -->
  <text x="60" y="48" text-anchor="middle" font-size="8" font-weight="bold" fill="#1e293b">tema claro</text>
  <rect x="20" y="58" width="80" height="80" fill="#f8fafc" stroke="#e2e8f0"/>
  <circle cx="60" cy="98" r="22" fill="#3b82f6"/>
  <path d="M 50 98 L 58 106 L 70 90" stroke="white" stroke-width="4" fill="none" stroke-linecap="round" stroke-linejoin="round"/>
  <text x="60" y="150" text-anchor="middle" font-size="6" fill="#64748b">visible ✓</text>

  <!-- Tema oscuro con colores fijos -->
  <text x="150" y="48" text-anchor="middle" font-size="8" font-weight="bold" fill="#1e293b">tema oscuro</text>
  <rect x="110" y="58" width="80" height="80" fill="#0f172a" stroke="#1e293b"/>
  <circle cx="150" cy="98" r="22" fill="#3b82f6"/>
  <path d="M 140 98 L 148 106 L 160 90" stroke="white" stroke-width="4" fill="none" stroke-linecap="round" stroke-linejoin="round"/>
  <text x="150" y="150" text-anchor="middle" font-size="6" fill="#64748b">funciona, pero...</text>

  <!-- Alto contraste -->
  <text x="240" y="48" text-anchor="middle" font-size="8" font-weight="bold" fill="#1e293b">alto contraste</text>
  <rect x="200" y="58" width="80" height="80" fill="black" stroke="white"/>
  <circle cx="240" cy="98" r="22" fill="#3b82f6"/>
  <path d="M 230 98 L 238 106 L 250 90" stroke="white" stroke-width="4" fill="none" stroke-linecap="round" stroke-linejoin="round"/>
  <text x="240" y="150" text-anchor="middle" font-size="6" fill="#ef4444">mal contraste con el azul fijo</text>

  <rect x="15" y="165" width="270" height="65" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="181" font-size="8" font-weight="bold" fill="#92400e">El problema:</text>
  <text x="25" y="194" font-size="7" fill="#475569">los colores fijos (#3b82f6) ignoran las preferencias del usuario.</text>
  <text x="25" y="208" font-size="7" fill="#475569">Con currentColor o media queries el SVG se adapta automáticamente</text>
  <text x="25" y="220" font-size="7" fill="#475569">y respeta el tema del sistema sin necesidad de duplicar archivos.</text>
</svg>
```

---

## `currentColor`: el truco más simple

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    currentColor hereda del CSS del padre
  </text>

  <!-- Explicación código -->
  <rect x="15" y="38" width="270" height="80" fill="#0f172a" rx="3"/>
  <text x="25" y="56" font-size="8" font-family="monospace" fill="#94a3b8">/* CSS del padre */</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#e2e8f0">button { color: #3b82f6; }</text>
  <text x="25" y="88" font-size="8" font-family="monospace" fill="#94a3b8">&lt;!-- SVG hereda ese color --&gt;</text>
  <text x="25" y="102" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;svg fill="currentColor"&gt;...&lt;/svg&gt;</text>

  <!-- Demo con tres botones de color distinto -->
  <rect x="15" y="130" width="270" height="95" fill="white" stroke="#cbd5e1" rx="3"/>

  <!-- Botón 1 -->
  <rect x="30" y="150" width="75" height="30" fill="#3b82f6" rx="4"/>
  <path d="M 45 160 L 50 170 L 60 155" stroke="white" stroke-width="2.5" fill="none" stroke-linecap="round"/>
  <text x="85" y="169" font-size="9" fill="white" font-weight="bold">OK</text>

  <!-- Botón 2 -->
  <rect x="113" y="150" width="75" height="30" fill="#10b981" rx="4"/>
  <path d="M 128 160 L 133 170 L 143 155" stroke="white" stroke-width="2.5" fill="none" stroke-linecap="round"/>
  <text x="158" y="169" font-size="9" fill="white" font-weight="bold">Save</text>

  <!-- Botón 3 -->
  <rect x="196" y="150" width="75" height="30" fill="#ef4444" rx="4"/>
  <path d="M 211 160 L 216 170 L 226 155" stroke="white" stroke-width="2.5" fill="none" stroke-linecap="round"/>
  <text x="240" y="169" font-size="9" fill="white" font-weight="bold">Del</text>

  <text x="150" y="202" text-anchor="middle" font-size="7" fill="#64748b">
    mismo SVG, cada botón impone su color vía CSS
  </text>
  <text x="150" y="215" text-anchor="middle" font-size="6" fill="#94a3b8">
    (compatible incluso con navegadores muy antiguos)
  </text>
</svg>
```

---

## `prefers-color-scheme` dentro del SVG

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    media query para modo oscuro
  </text>

  <!-- Bloque de código -->
  <rect x="15" y="38" width="270" height="160" fill="#0f172a" rx="3"/>
  <text x="25" y="56" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;svg viewBox="0 0 100 100"&gt;</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#e2e8f0">  &lt;style&gt;</text>
  <text x="35" y="84" font-size="8" font-family="monospace" fill="#94a3b8">    /* modo claro por defecto */</text>
  <text x="35" y="98" font-size="8" font-family="monospace" fill="#60a5fa">    circle {</text>
  <text x="45" y="110" font-size="8" font-family="monospace" fill="#60a5fa">      fill: #1a1a2e;</text>
  <text x="45" y="122" font-size="8" font-family="monospace" fill="#60a5fa">      stroke: #e0e0e0;</text>
  <text x="35" y="134" font-size="8" font-family="monospace" fill="#60a5fa">    }</text>
  <text x="35" y="150" font-size="8" font-family="monospace" fill="#fbbf24">    @media (prefers-color-scheme: dark) {</text>
  <text x="45" y="162" font-size="8" font-family="monospace" fill="#fbbf24">      circle {</text>
  <text x="55" y="174" font-size="8" font-family="monospace" fill="#fbbf24">        fill: #e0e0e0;</text>
  <text x="55" y="186" font-size="8" font-family="monospace" fill="#fbbf24">        stroke: #1a1a2e;</text>
  <text x="45" y="198" font-size="8" font-family="monospace" fill="#fbbf24">      }</text>

  <!-- Resultado visual -->
  <text x="75" y="220" text-anchor="middle" font-size="7" fill="#64748b">modo claro</text>
  <rect x="30" y="224" width="90" height="28" fill="#f8fafc" stroke="#e2e8f0"/>
  <circle cx="75" cy="238" r="10" fill="#1a1a2e" stroke="#e0e0e0" stroke-width="2"/>

  <text x="225" y="220" text-anchor="middle" font-size="7" fill="#64748b">modo oscuro</text>
  <rect x="180" y="224" width="90" height="28" fill="#0f172a" stroke="#1e293b"/>
  <circle cx="225" cy="238" r="10" fill="#e0e0e0" stroke="#1a1a2e" stroke-width="2"/>
</svg>
```

---

## Variables CSS: el patrón más escalable

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    variables CSS para temas
  </text>

  <!-- Bloque código -->
  <rect x="15" y="38" width="270" height="210" fill="#0f172a" rx="3"/>
  <text x="25" y="56" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;svg viewBox="..."&gt;</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#e2e8f0">  &lt;style&gt;</text>

  <text x="35" y="84" font-size="8" font-family="monospace" fill="#60a5fa">    :root {</text>
  <text x="45" y="96" font-size="8" font-family="monospace" fill="#60a5fa">      --c-fondo: white;</text>
  <text x="45" y="108" font-size="8" font-family="monospace" fill="#60a5fa">      --c-acento: #005ea5;</text>
  <text x="35" y="120" font-size="8" font-family="monospace" fill="#60a5fa">    }</text>

  <text x="35" y="136" font-size="8" font-family="monospace" fill="#fbbf24">    @media (prefers-color-scheme: dark) {</text>
  <text x="45" y="148" font-size="8" font-family="monospace" fill="#fbbf24">      :root {</text>
  <text x="55" y="160" font-size="8" font-family="monospace" fill="#fbbf24">        --c-fondo: #1a1a2e;</text>
  <text x="55" y="172" font-size="8" font-family="monospace" fill="#fbbf24">        --c-acento: #7ab3d9;</text>
  <text x="45" y="184" font-size="8" font-family="monospace" fill="#fbbf24">      }</text>
  <text x="35" y="196" font-size="8" font-family="monospace" fill="#fbbf24">    }</text>

  <text x="35" y="212" font-size="8" font-family="monospace" fill="#34d399">    circle {</text>
  <text x="45" y="224" font-size="8" font-family="monospace" fill="#34d399">      fill: var(--c-fondo);</text>
  <text x="45" y="236" font-size="8" font-family="monospace" fill="#34d399">      stroke: var(--c-acento);</text>
  <text x="35" y="248" font-size="8" font-family="monospace" fill="#34d399">    }</text>
</svg>
```

---

## Cuándo elegir cada enfoque

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    guía rápida de decisión
  </text>

  <!-- currentColor -->
  <rect x="15" y="38" width="270" height="60" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#1e40af">currentColor</text>
  <text x="25" y="68" font-size="7" fill="#475569">• íconos monocromáticos dentro de botones o enlaces</text>
  <text x="25" y="80" font-size="7" fill="#475569">• heredan el color del texto del padre (CSS color)</text>
  <text x="25" y="92" font-size="7" fill="#1e40af">máxima compatibilidad, sin media queries</text>

  <!-- prefers-color-scheme -->
  <rect x="15" y="106" width="270" height="60" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="122" font-size="9" font-weight="bold" fill="#166534">prefers-color-scheme</text>
  <text x="25" y="136" font-size="7" fill="#475569">• SVG multicolor que debe tener dos versiones de paleta</text>
  <text x="25" y="148" font-size="7" fill="#475569">• ilustraciones, diagramas, gráficos con fondo/borde distintos</text>
  <text x="25" y="160" font-size="7" fill="#166534">cambia automáticamente según preferencia del sistema</text>

  <!-- variables CSS -->
  <rect x="15" y="174" width="270" height="60" fill="#ede9fe" stroke="#8b5cf6" rx="3"/>
  <text x="25" y="190" font-size="9" font-weight="bold" fill="#5b21b6">variables CSS</text>
  <text x="25" y="204" font-size="7" fill="#475569">• SVGs con muchos colores relacionados a un sistema de diseño</text>
  <text x="25" y="216" font-size="7" fill="#475569">• cuando el SVG hereda variables del documento HTML</text>
  <text x="25" y="228" font-size="7" fill="#5b21b6">cambios centralizados, ideal para design systems</text>

  <!-- Información adicional -->
  <rect x="15" y="242" width="270" height="32" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="258" font-size="7" font-weight="bold" fill="#92400e">Principio general de accesibilidad:</text>
  <text x="25" y="270" font-size="7" fill="#475569">nunca transmitir información solo por color (forma + texto + color)</text>
</svg>
```
