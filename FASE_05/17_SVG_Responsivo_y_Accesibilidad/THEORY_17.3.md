# 17.3 SVG adaptativo con media queries

Un SVG **adaptativo** no solo escala: también **cambia su contenido** según el espacio disponible. Oculta detalles en tamaños pequeños, muestra etiquetas en grandes, reorganiza ejes. Todo mediante media queries dentro del propio SVG.

---

## Media queries dentro del SVG

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    &lt;style&gt; con media queries dentro del SVG
  </text>

  <!-- Bloque de código -->
  <rect x="15" y="38" width="270" height="170" fill="#0f172a" rx="3"/>
  <text x="25" y="56" font-size="8" font-family="monospace" fill="#94a3b8">&lt;svg viewBox="0 0 200 100"&gt;</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#e2e8f0">  &lt;style&gt;</text>
  <text x="25" y="84" font-size="8" font-family="monospace" fill="#60a5fa">    .detalle { display: block; }</text>
  <text x="25" y="98" font-size="8" font-family="monospace" fill="#fbbf24">    @media (max-width: 200px) {</text>
  <text x="25" y="112" font-size="8" font-family="monospace" fill="#fbbf24">      .detalle { display: none; }</text>
  <text x="25" y="126" font-size="8" font-family="monospace" fill="#fbbf24">    }</text>
  <text x="25" y="140" font-size="8" font-family="monospace" fill="#e2e8f0">  &lt;/style&gt;</text>
  <text x="25" y="154" font-size="8" font-family="monospace" fill="#34d399">  &lt;circle cx="50" cy="50" r="40"/&gt;</text>
  <text x="25" y="168" font-size="8" font-family="monospace" fill="#34d399">  &lt;text class="detalle" ...&gt;Detalle&lt;/text&gt;</text>
  <text x="25" y="182" font-size="8" font-family="monospace" fill="#94a3b8">&lt;/svg&gt;</text>
  <text x="25" y="200" font-size="7" fill="#64748b">el SVG lleva su propio bloque &lt;style&gt; como si fuera CSS externo</text>
</svg>
```

---

## El detalle crítico: a qué responden las media queries

```svg
<svg width="300" height="250"
     viewBox="0 0 300 250"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    ¿a qué ancho responden las media queries?
  </text>

  <!-- SVG inline -->
  <rect x="15" y="38" width="270" height="88" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#166534">SVG inline en HTML</text>
  <text x="25" y="68" font-size="7" fill="#475569">las @media responden al <tspan font-weight="bold">viewport del navegador</tspan></text>
  <text x="25" y="82" font-size="7" fill="#475569">• @media (max-width: 600px) → verdadero si la ventana mide 600px</text>
  <text x="25" y="95" font-size="7" fill="#475569">• se comporta como cualquier CSS del documento HTML</text>
  <text x="25" y="112" font-size="7" font-family="monospace" fill="#166534">&lt;div&gt;&lt;svg&gt;...&lt;/svg&gt;&lt;/div&gt;  ← CSS global del documento</text>

  <!-- SVG externo -->
  <rect x="15" y="134" width="270" height="100" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="150" font-size="9" font-weight="bold" fill="#1e40af">SVG externo (img/object/background)</text>
  <text x="25" y="164" font-size="7" fill="#475569">las @media responden al <tspan font-weight="bold">viewport del propio SVG</tspan></text>
  <text x="25" y="178" font-size="7" fill="#475569">• @media (max-width: 200px) → verdadero si el SVG mide 200px</text>
  <text x="25" y="191" font-size="7" fill="#475569">• aunque la pantalla sea de 1920px, lo que importa es el SVG</text>
  <text x="25" y="208" font-size="7" font-family="monospace" fill="#1e40af">&lt;img src="logo.svg" width="200"&gt;  ← 200px activa móvil</text>
  <text x="25" y="224" font-size="7" fill="#1e40af">esto es útil: cada instancia del SVG se adapta a su tamaño real</text>
</svg>
```

---

## Demostración: logo adaptativo

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">el mismo logo en tres tamaños</text>

  <!-- Tamaño grande: logo + texto -->
  <rect x="15" y="35" width="270" height="60" fill="white" stroke="#cbd5e1" rx="3"/>
  <text x="25" y="50" font-size="7" font-weight="bold" fill="#64748b">grande (&gt; 400px): logo completo</text>
  <circle cx="50" cy="75" r="14" fill="#3b82f6"/>
  <path d="M 42 75 L 48 81 L 58 69" stroke="white" stroke-width="2.5" fill="none" stroke-linecap="round"/>
  <text x="75" y="72" font-size="11" font-weight="bold" fill="#1e293b">Acme Corp</text>
  <text x="75" y="85" font-size="7" fill="#64748b">Soluciones empresariales</text>

  <!-- Tamaño mediano: logo + nombre -->
  <rect x="15" y="103" width="270" height="50" fill="white" stroke="#cbd5e1" rx="3"/>
  <text x="25" y="118" font-size="7" font-weight="bold" fill="#64748b">mediano (200-400px): sin tagline</text>
  <circle cx="50" cy="138" r="11" fill="#3b82f6"/>
  <path d="M 44 138 L 49 143 L 57 133" stroke="white" stroke-width="2" fill="none" stroke-linecap="round"/>
  <text x="68" y="142" font-size="9" font-weight="bold" fill="#1e293b">Acme Corp</text>

  <!-- Tamaño pequeño: solo ícono -->
  <rect x="15" y="161" width="270" height="50" fill="white" stroke="#cbd5e1" rx="3"/>
  <text x="25" y="176" font-size="7" font-weight="bold" fill="#64748b">pequeño (&lt; 200px): solo el símbolo</text>
  <circle cx="50" cy="196" r="10" fill="#3b82f6"/>
  <path d="M 44 196 L 48 200 L 56 192" stroke="white" stroke-width="2" fill="none" stroke-linecap="round"/>

  <text x="150" y="228" text-anchor="middle" font-size="7" fill="#64748b">
    un solo archivo SVG con @media decide qué mostrar
  </text>
  <text x="150" y="242" text-anchor="middle" font-size="7" fill="#94a3b8">
    .tagline, .nombre con display: none en los breakpoints pequeños
  </text>
</svg>
```

---

## Simplificar en tamaños pequeños

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">gráfico con etiquetas condicionales</text>

  <!-- Versión grande: con etiquetas -->
  <rect x="15" y="35" width="130" height="170" fill="white" stroke="#cbd5e1" rx="3"/>
  <text x="80" y="50" text-anchor="middle" font-size="7" font-weight="bold" fill="#64748b">grande: todas las etiquetas</text>

  <!-- Ejes -->
  <line x1="30" y1="160" x2="130" y2="160" stroke="#94a3b8"/>
  <line x1="30" y1="70" x2="30" y2="160" stroke="#94a3b8"/>

  <!-- Barras -->
  <rect x="40" y="130" width="16" height="30" fill="#3b82f6"/>
  <text x="48" y="127" text-anchor="middle" font-size="6" fill="#1e293b">45</text>
  <text x="48" y="172" text-anchor="middle" font-size="6" fill="#64748b">Ene</text>

  <rect x="62" y="110" width="16" height="50" fill="#3b82f6"/>
  <text x="70" y="107" text-anchor="middle" font-size="6" fill="#1e293b">62</text>
  <text x="70" y="172" text-anchor="middle" font-size="6" fill="#64748b">Feb</text>

  <rect x="84" y="90" width="16" height="70" fill="#3b82f6"/>
  <text x="92" y="87" text-anchor="middle" font-size="6" fill="#1e293b">78</text>
  <text x="92" y="172" text-anchor="middle" font-size="6" fill="#64748b">Mar</text>

  <rect x="106" y="80" width="16" height="80" fill="#3b82f6"/>
  <text x="114" y="77" text-anchor="middle" font-size="6" fill="#1e293b">91</text>
  <text x="114" y="172" text-anchor="middle" font-size="6" fill="#64748b">Abr</text>

  <text x="80" y="195" text-anchor="middle" font-size="6" fill="#475569">Ventas trimestrales</text>

  <!-- Versión pequeña: sin etiquetas -->
  <rect x="155" y="35" width="130" height="170" fill="white" stroke="#cbd5e1" rx="3"/>
  <text x="220" y="50" text-anchor="middle" font-size="7" font-weight="bold" fill="#64748b">pequeño: solo lo esencial</text>

  <line x1="170" y1="160" x2="270" y2="160" stroke="#94a3b8"/>

  <rect x="180" y="130" width="16" height="30" fill="#3b82f6"/>
  <rect x="202" y="110" width="16" height="50" fill="#3b82f6"/>
  <rect x="224" y="90" width="16" height="70" fill="#3b82f6"/>
  <rect x="246" y="80" width="16" height="80" fill="#3b82f6"/>

  <text x="220" y="195" text-anchor="middle" font-size="6" fill="#475569">Ventas trimestrales</text>
</svg>
```

---

## Casos de uso habituales

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    ¿cuándo conviene un SVG adaptativo?
  </text>

  <!-- Caso 1 -->
  <rect x="15" y="38" width="270" height="48" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#1e40af">Logos con variantes</text>
  <text x="25" y="68" font-size="7" fill="#475569">grande: símbolo + nombre + tagline</text>
  <text x="25" y="80" font-size="7" fill="#475569">pequeño: solo el símbolo</text>

  <!-- Caso 2 -->
  <rect x="15" y="92" width="270" height="48" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="108" font-size="9" font-weight="bold" fill="#166534">Gráficos de datos</text>
  <text x="25" y="122" font-size="7" fill="#475569">grande: todos los puntos + etiquetas + leyenda</text>
  <text x="25" y="134" font-size="7" fill="#475569">pequeño: solo los valores clave, sin ejes auxiliares</text>

  <!-- Caso 3 -->
  <rect x="15" y="146" width="270" height="48" fill="#ede9fe" stroke="#8b5cf6" rx="3"/>
  <text x="25" y="162" font-size="9" font-weight="bold" fill="#5b21b6">Diagramas técnicos</text>
  <text x="25" y="176" font-size="7" fill="#475569">grande: anotaciones detalladas de cada componente</text>
  <text x="25" y="188" font-size="7" fill="#475569">pequeño: solo la estructura principal, sin texto</text>

  <!-- Caso 4 -->
  <rect x="15" y="200" width="270" height="48" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="216" font-size="9" font-weight="bold" fill="#92400e">Íconos semánticos</text>
  <text x="25" y="230" font-size="7" fill="#475569">grande: ícono con fondo + borde + decoración</text>
  <text x="25" y="242" font-size="7" fill="#475569">pequeño: solo la silueta limpia</text>
</svg>
```
