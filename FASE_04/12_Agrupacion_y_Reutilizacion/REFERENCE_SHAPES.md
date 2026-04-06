# Cheat sheet — Agrupación y Reutilización

---

## Los 4 elementos en una tabla visual

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" fill="#f8fafc" font-weight="bold">
    &lt;g&gt; · &lt;use&gt; · &lt;symbol&gt; · &lt;defs&gt;
  </text>

  <!-- Fila g -->
  <rect x="10" y="34" width="60" height="36" fill="#dbeafe" stroke="#3b82f6" stroke-width="1.5" rx="4"/>
  <text x="40" y="50" text-anchor="middle" font-size="10" font-weight="bold" fill="#1e40af">&lt;g&gt;</text>
  <text x="40" y="63" text-anchor="middle" font-size="7" fill="#64748b">grupo</text>
  <text x="78" y="48" font-size="7" fill="#1e293b">Agrupa elementos. Hereda fill, stroke,</text>
  <text x="78" y="58" font-size="7" fill="#1e293b">font-*. Permite transform común.</text>
  <text x="78" y="68" font-size="7" fill="#64748b" font-style="italic">Como un div: contenedor lógico</text>

  <!-- Fila use -->
  <rect x="10" y="78" width="60" height="36" fill="#dcfce7" stroke="#10b981" stroke-width="1.5" rx="4"/>
  <text x="40" y="94" text-anchor="middle" font-size="10" font-weight="bold" fill="#166534">&lt;use&gt;</text>
  <text x="40" y="107" text-anchor="middle" font-size="7" fill="#64748b">instancia</text>
  <text x="78" y="92"  font-size="7" fill="#1e293b">Duplica un elemento definido con id.</text>
  <text x="78" y="102" font-size="7" fill="#1e293b">href="#id", x, y, width, height.</text>
  <text x="78" y="112" font-size="7" fill="#64748b" font-style="italic">Definir una, usar muchas</text>

  <!-- Fila symbol -->
  <rect x="10" y="122" width="60" height="36" fill="#fef9c3" stroke="#f59e0b" stroke-width="1.5" rx="4"/>
  <text x="40" y="138" text-anchor="middle" font-size="10" font-weight="bold" fill="#92400e">&lt;symbol&gt;</text>
  <text x="40" y="151" text-anchor="middle" font-size="7" fill="#64748b">plantilla</text>
  <text x="78" y="136" font-size="7" fill="#1e293b">Como g pero con viewBox propio.</text>
  <text x="78" y="146" font-size="7" fill="#1e293b">No se dibuja solo — requiere &lt;use&gt;.</text>
  <text x="78" y="156" font-size="7" fill="#64748b" font-style="italic">La base de cualquier sistema de iconos</text>

  <!-- Fila defs -->
  <rect x="10" y="166" width="60" height="36" fill="#fce7f3" stroke="#ec4899" stroke-width="1.5" rx="4"/>
  <text x="40" y="182" text-anchor="middle" font-size="10" font-weight="bold" fill="#9d174d">&lt;defs&gt;</text>
  <text x="40" y="195" text-anchor="middle" font-size="7" fill="#64748b">biblioteca</text>
  <text x="78" y="180" font-size="7" fill="#1e293b">Contenedor de definiciones no visibles:</text>
  <text x="78" y="190" font-size="7" fill="#1e293b">gradients, patterns, filters, symbols…</text>
  <text x="78" y="200" font-size="7" fill="#64748b" font-style="italic">Todo lo que hay que referenciar</text>

  <text x="150" y="215" text-anchor="middle" font-size="7" fill="#94a3b8">
    los 4 juntos = arquitectura limpia y mantenible
  </text>
</svg>
```

---

## Herencia: qué se propaga y qué no

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" fill="#f8fafc" font-weight="bold">
    Herencia en SVG
  </text>

  <!-- Columna izquierda: heredables -->
  <rect x="10" y="34" width="135" height="175" fill="#dcfce7" stroke="#10b981" stroke-width="1.5" rx="4"/>
  <text x="77" y="52" text-anchor="middle" font-size="10" fill="#166534" font-weight="bold">✓ Heredables</text>

  <text x="20" y="72"  font-size="7" font-weight="bold" fill="#1e293b">Pintura</text>
  <text x="20" y="84"  font-size="7" fill="#64748b">fill, fill-opacity, fill-rule</text>
  <text x="20" y="96"  font-size="7" fill="#64748b">stroke, stroke-width</text>
  <text x="20" y="108" font-size="7" fill="#64748b">stroke-opacity, stroke-dasharray</text>
  <text x="20" y="120" font-size="7" fill="#64748b">stroke-linecap, stroke-linejoin</text>

  <text x="20" y="138" font-size="7" font-weight="bold" fill="#1e293b">Texto</text>
  <text x="20" y="150" font-size="7" fill="#64748b">font-family, font-size</text>
  <text x="20" y="162" font-size="7" fill="#64748b">font-weight, font-style</text>
  <text x="20" y="174" font-size="7" fill="#64748b">text-anchor, letter-spacing</text>

  <text x="20" y="192" font-size="7" font-weight="bold" fill="#1e293b">Otros</text>
  <text x="20" y="204" font-size="7" fill="#64748b">color, visibility, cursor</text>

  <!-- Columna derecha: no heredables -->
  <rect x="155" y="34" width="135" height="175" fill="#fef2f2" stroke="#ef4444" stroke-width="1.5" rx="4"/>
  <text x="222" y="52" text-anchor="middle" font-size="10" fill="#9d174d" font-weight="bold">× No heredables</text>

  <text x="165" y="72"  font-size="7" font-weight="bold" fill="#1e293b">Geometría</text>
  <text x="165" y="84"  font-size="7" fill="#64748b">x, y, width, height</text>
  <text x="165" y="96"  font-size="7" fill="#64748b">cx, cy, r, rx, ry</text>
  <text x="165" y="108" font-size="7" fill="#64748b">x1, y1, x2, y2</text>
  <text x="165" y="120" font-size="7" fill="#64748b">points, d (de paths)</text>

  <text x="165" y="138" font-size="7" font-weight="bold" fill="#1e293b">Efectos</text>
  <text x="165" y="150" font-size="7" fill="#64748b">opacity (al grupo)</text>
  <text x="165" y="162" font-size="7" fill="#64748b">clip-path, mask, filter</text>
  <text x="165" y="174" font-size="7" fill="#64748b">display, overflow</text>

  <text x="165" y="192" font-size="7" font-weight="bold" fill="#1e293b">Transform</text>
  <text x="165" y="204" font-size="7" fill="#64748b">transform (se compone)</text>
</svg>
```

---

## `<g>` vs `<symbol>` vs `<use>`: decisiones rápidas

```svg
<svg width="300" height="200"
     viewBox="0 0 300 200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" fill="#f8fafc" font-weight="bold">
    Qué elemento usar
  </text>

  <!-- Pregunta 1 -->
  <g transform="translate(15, 40)">
    <rect x="0" y="0" width="270" height="35" fill="#ede9fe" stroke="#8b5cf6" stroke-width="1.5" rx="4"/>
    <text x="10" y="16" font-size="8" font-weight="bold" fill="#4c1d95">¿Necesito reutilizar en varios tamaños?</text>
    <text x="10" y="28" font-size="7" fill="#1e293b">→ <tspan font-weight="bold" fill="#4c1d95">&lt;symbol&gt;</tspan> con viewBox + &lt;use width height&gt;</text>
  </g>

  <!-- Pregunta 2 -->
  <g transform="translate(15, 83)">
    <rect x="0" y="0" width="270" height="35" fill="#ede9fe" stroke="#8b5cf6" stroke-width="1.5" rx="4"/>
    <text x="10" y="16" font-size="8" font-weight="bold" fill="#4c1d95">¿Quiero copiar un elemento exacto varias veces?</text>
    <text x="10" y="28" font-size="7" fill="#1e293b">→ <tspan font-weight="bold" fill="#4c1d95">&lt;g id&gt;</tspan> + varios &lt;use href="#id"&gt;</text>
  </g>

  <!-- Pregunta 3 -->
  <g transform="translate(15, 126)">
    <rect x="0" y="0" width="270" height="35" fill="#ede9fe" stroke="#8b5cf6" stroke-width="1.5" rx="4"/>
    <text x="10" y="16" font-size="8" font-weight="bold" fill="#4c1d95">¿Solo quiero agrupar para transformar o heredar estilo?</text>
    <text x="10" y="28" font-size="7" fill="#1e293b">→ <tspan font-weight="bold" fill="#4c1d95">&lt;g&gt;</tspan> con transform o atributos de presentación</text>
  </g>

  <!-- Pregunta 4 -->
  <g transform="translate(15, 169)">
    <rect x="0" y="0" width="270" height="25" fill="#ede9fe" stroke="#8b5cf6" stroke-width="1.5" rx="4"/>
    <text x="10" y="12" font-size="8" font-weight="bold" fill="#4c1d95">¿Es un gradient / pattern / filter / clipPath?</text>
    <text x="10" y="21" font-size="7" fill="#1e293b">→ dentro de <tspan font-weight="bold" fill="#4c1d95">&lt;defs&gt;</tspan></text>
  </g>
</svg>
```

---

## Patrones de estilizado de `<use>`

```svg
<svg width="300" height="210"
     viewBox="0 0 300 210"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <!-- símbolo sin fill fijo, usando currentColor -->
    <symbol id="cheat-star" viewBox="0 0 24 24">
      <polygon points="12,2 15,9 22,10 16,15 18,22 12,18 6,22 8,15 2,10 9,9" fill="currentColor"/>
    </symbol>

    <!-- símbolo con variables CSS -->
    <symbol id="cheat-btn" viewBox="0 0 24 24">
      <rect x="2" y="2" width="20" height="20" rx="3"
            fill="var(--btn-bg, #3b82f6)"
            stroke="var(--btn-border, #1e40af)" stroke-width="2"/>
    </symbol>
  </defs>

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" fill="#f8fafc" font-weight="bold">
    Estilizando instancias &lt;use&gt;
  </text>

  <!-- Opción 1: currentColor -->
  <rect x="10" y="34" width="280" height="55" fill="#dbeafe" stroke="#3b82f6" stroke-width="1.5" rx="4"/>
  <text x="20" y="50" font-size="8" font-weight="bold" fill="#1e40af">1. currentColor (un color)</text>

  <use href="#cheat-star" x="20" y="58" width="24" height="24" style="color:#ef4444"/>
  <use href="#cheat-star" x="50" y="58" width="24" height="24" style="color:#f59e0b"/>
  <use href="#cheat-star" x="80" y="58" width="24" height="24" style="color:#10b981"/>
  <use href="#cheat-star" x="110" y="58" width="24" height="24" style="color:#3b82f6"/>
  <use href="#cheat-star" x="140" y="58" width="24" height="24" style="color:#8b5cf6"/>
  <text x="180" y="75" font-size="7" fill="#64748b">fill="currentColor" + color en &lt;use&gt;</text>

  <!-- Opción 2: variables CSS -->
  <rect x="10" y="97" width="280" height="55" fill="#dcfce7" stroke="#10b981" stroke-width="1.5" rx="4"/>
  <text x="20" y="113" font-size="8" font-weight="bold" fill="#166534">2. Variables CSS (múltiples valores)</text>

  <use href="#cheat-btn" x="20" y="120" width="28" height="28" style="--btn-bg:#fce7f3;--btn-border:#9d174d"/>
  <use href="#cheat-btn" x="55" y="120" width="28" height="28" style="--btn-bg:#fef9c3;--btn-border:#92400e"/>
  <use href="#cheat-btn" x="90" y="120" width="28" height="28" style="--btn-bg:#ede9fe;--btn-border:#4c1d95"/>
  <text x="180" y="138" font-size="7" fill="#64748b">var(--x) permite N colores</text>

  <!-- Opción 3: atributos heredados -->
  <rect x="10" y="160" width="280" height="42" fill="#fef9c3" stroke="#f59e0b" stroke-width="1.5" rx="4"/>
  <text x="20" y="176" font-size="8" font-weight="bold" fill="#92400e">3. fill en &lt;use&gt; (si el símbolo no lo tiene)</text>
  <text x="20" y="192" font-size="7" fill="#64748b">los hijos sin fill heredan del &lt;use fill="..."&gt;</text>
</svg>
```
