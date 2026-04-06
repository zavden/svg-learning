# 18.3 SVG como fondo CSS

Un SVG referenciado con `background-image` se convierte en una **textura CSS**: controlable con `background-size`, `background-repeat` y `background-position`. Ideal para patrones decorativos y separadores, pero **completamente invisible** para accesibilidad y manipulación.

---

## Sintaxis

```svg
<svg width="300" height="180"
     viewBox="0 0 300 180"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    background-image con SVG
  </text>

  <rect x="15" y="38" width="270" height="130" fill="#0f172a" rx="3"/>
  <text x="25" y="56" font-size="8" font-family="monospace" fill="#e2e8f0">.fondo {</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#60a5fa">  background-image: url('patron.svg');</text>
  <text x="25" y="84" font-size="8" font-family="monospace" fill="#fbbf24">  background-size: 50px 50px;</text>
  <text x="25" y="98" font-size="8" font-family="monospace" fill="#fbbf24">  background-repeat: repeat;</text>
  <text x="25" y="112" font-size="8" font-family="monospace" fill="#fbbf24">  background-position: center;</text>
  <text x="25" y="126" font-size="8" font-family="monospace" fill="#e2e8f0">}</text>
  <text x="25" y="148" font-size="8" font-family="monospace" fill="#94a3b8">/* el SVG se carga como imagen externa */</text>
  <text x="25" y="160" font-size="8" font-family="monospace" fill="#94a3b8">/* misma caja sandboxed que en &lt;img&gt; */</text>
</svg>
```

---

## Patrón decorativo repetido

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    un solo tile SVG puede cubrir cualquier área
  </text>

  <!-- Tile -->
  <text x="60" y="48" text-anchor="middle" font-size="8" font-weight="bold" fill="#1e293b">tile (20×20)</text>
  <rect x="35" y="55" width="50" height="50" fill="white" stroke="#3b82f6" stroke-width="1.5"/>
  <circle cx="60" cy="80" r="8" fill="none" stroke="#3b82f6" stroke-width="1.5"/>
  <line x1="52" y1="72" x2="68" y2="88" stroke="#3b82f6" stroke-width="1"/>

  <!-- Aplicado -->
  <text x="210" y="48" text-anchor="middle" font-size="8" font-weight="bold" fill="#1e293b">repetido en 180×50</text>
  <rect x="125" y="55" width="170" height="50" fill="white" stroke="#cbd5e1"/>

  <!-- Simulación del patrón repetido -->
  <circle cx="135" cy="65" r="3" fill="none" stroke="#3b82f6"/>
  <circle cx="155" cy="65" r="3" fill="none" stroke="#3b82f6"/>
  <circle cx="175" cy="65" r="3" fill="none" stroke="#3b82f6"/>
  <circle cx="195" cy="65" r="3" fill="none" stroke="#3b82f6"/>
  <circle cx="215" cy="65" r="3" fill="none" stroke="#3b82f6"/>
  <circle cx="235" cy="65" r="3" fill="none" stroke="#3b82f6"/>
  <circle cx="255" cy="65" r="3" fill="none" stroke="#3b82f6"/>
  <circle cx="275" cy="65" r="3" fill="none" stroke="#3b82f6"/>
  <circle cx="135" cy="85" r="3" fill="none" stroke="#3b82f6"/>
  <circle cx="155" cy="85" r="3" fill="none" stroke="#3b82f6"/>
  <circle cx="175" cy="85" r="3" fill="none" stroke="#3b82f6"/>
  <circle cx="195" cy="85" r="3" fill="none" stroke="#3b82f6"/>
  <circle cx="215" cy="85" r="3" fill="none" stroke="#3b82f6"/>
  <circle cx="235" cy="85" r="3" fill="none" stroke="#3b82f6"/>
  <circle cx="255" cy="85" r="3" fill="none" stroke="#3b82f6"/>
  <circle cx="275" cy="85" r="3" fill="none" stroke="#3b82f6"/>
  <circle cx="135" cy="100" r="3" fill="none" stroke="#3b82f6"/>
  <circle cx="155" cy="100" r="3" fill="none" stroke="#3b82f6"/>
  <circle cx="175" cy="100" r="3" fill="none" stroke="#3b82f6"/>
  <circle cx="195" cy="100" r="3" fill="none" stroke="#3b82f6"/>
  <circle cx="215" cy="100" r="3" fill="none" stroke="#3b82f6"/>
  <circle cx="235" cy="100" r="3" fill="none" stroke="#3b82f6"/>
  <circle cx="255" cy="100" r="3" fill="none" stroke="#3b82f6"/>
  <circle cx="275" cy="100" r="3" fill="none" stroke="#3b82f6"/>

  <!-- CSS -->
  <rect x="15" y="125" width="270" height="98" fill="#0f172a" rx="3"/>
  <text x="25" y="142" font-size="8" font-family="monospace" fill="#94a3b8">/* un tile reutilizado infinitamente */</text>
  <text x="25" y="156" font-size="8" font-family="monospace" fill="#e2e8f0">section {</text>
  <text x="25" y="170" font-size="8" font-family="monospace" fill="#60a5fa">  background-image: url('dots.svg');</text>
  <text x="25" y="184" font-size="8" font-family="monospace" fill="#fbbf24">  background-size: 20px 20px;</text>
  <text x="25" y="198" font-size="8" font-family="monospace" fill="#fbbf24">  background-repeat: repeat;</text>
  <text x="25" y="212" font-size="8" font-family="monospace" fill="#e2e8f0">}</text>
</svg>
```

---

## Limitaciones heredadas

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    mismas restricciones que &lt;img&gt;... y una más
  </text>

  <!-- Restricciones -->
  <rect x="15" y="38" width="270" height="120" fill="#fee2e2" stroke="#ef4444" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#991b1b">✗ Restricciones</text>
  <text x="25" y="72" font-size="7" fill="#1e293b">• sin acceso al DOM interno del SVG</text>
  <text x="25" y="86" font-size="7" fill="#1e293b">• CSS externo no llega al contenido del SVG</text>
  <text x="25" y="100" font-size="7" fill="#1e293b">• scripts internos no se ejecutan</text>
  <text x="25" y="114" font-size="7" fill="#1e293b">• los eventos click van al elemento HTML contenedor</text>
  <text x="25" y="128" font-size="7" font-weight="bold" fill="#991b1b">• accesibilidad: CERO</text>
  <text x="25" y="142" font-size="7" fill="#991b1b">  los lectores de pantalla ignoran background-image</text>

  <!-- Regla -->
  <rect x="15" y="168" width="270" height="58" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="184" font-size="8" font-weight="bold" fill="#92400e">Regla absoluta</text>
  <text x="25" y="200" font-size="7" fill="#475569">si el SVG transmite información → NO lo uses como background.</text>
  <text x="25" y="214" font-size="7" fill="#475569">solo para decoración pura: patrones, texturas, separadores.</text>
</svg>
```

---

## Data URI en CSS: colores parametrizables

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    SVG como data URI en CSS
  </text>

  <!-- Código -->
  <rect x="15" y="38" width="270" height="160" fill="#0f172a" rx="3"/>
  <text x="25" y="56" font-size="8" font-family="monospace" fill="#94a3b8">/* ícono check en gris */</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#e2e8f0">.check {</text>
  <text x="25" y="84" font-size="8" font-family="monospace" fill="#60a5fa">  background-image: url("data:image/svg+xml,</text>
  <text x="25" y="96" font-size="7" font-family="monospace" fill="#94a3b8">    %3Csvg xmlns='...' viewBox='0 0 10 10'%3E</text>
  <text x="25" y="108" font-size="7" font-family="monospace" fill="#94a3b8">    %3Cpath d='M2 5L4 7L8 3' stroke='%23666'/%3E</text>
  <text x="25" y="120" font-size="7" font-family="monospace" fill="#94a3b8">    %3C/svg%3E");</text>
  <text x="25" y="132" font-size="8" font-family="monospace" fill="#e2e8f0">}</text>

  <text x="25" y="150" font-size="8" font-family="monospace" fill="#94a3b8">/* mismo ícono en azul cuando hover */</text>
  <text x="25" y="164" font-size="8" font-family="monospace" fill="#e2e8f0">.check:hover {</text>
  <text x="25" y="178" font-size="8" font-family="monospace" fill="#fbbf24">  background-image: url("data:image/svg+xml,</text>
  <text x="25" y="190" font-size="7" font-family="monospace" fill="#fbbf24">    ...stroke='%233b82f6'...");</text>

  <!-- Nota -->
  <rect x="15" y="208" width="270" height="42" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="224" font-size="7" fill="#1e40af">útil para estados: normal/hover/active/disabled</text>
  <text x="25" y="236" font-size="7" fill="#1e40af">con colores distintos generados por build tools o CSS-in-JS</text>
</svg>
```

---

## Cuándo conviene

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    ¿background-image de SVG?
  </text>

  <!-- Sí -->
  <rect x="15" y="38" width="270" height="88" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#166534">✓ Sí, para</text>
  <text x="25" y="70" font-size="7" fill="#1e293b">• patrones decorativos repetidos (puntos, líneas, texturas)</text>
  <text x="25" y="84" font-size="7" fill="#1e293b">• separadores y divisores visuales</text>
  <text x="25" y="98" font-size="7" fill="#1e293b">• íconos de pseudo-elementos (::before, ::after)</text>
  <text x="25" y="112" font-size="7" fill="#1e293b">• estados visuales sin interactividad (bullets de lista, flechas)</text>

  <!-- No -->
  <rect x="15" y="136" width="270" height="92" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="152" font-size="9" font-weight="bold" fill="#991b1b">✗ No, para</text>
  <text x="25" y="168" font-size="7" fill="#1e293b">• íconos que transmiten significado (usa &lt;img&gt; o inline)</text>
  <text x="25" y="182" font-size="7" fill="#1e293b">• logotipos (necesitan alt accesible)</text>
  <text x="25" y="196" font-size="7" fill="#1e293b">• gráficos de datos</text>
  <text x="25" y="210" font-size="7" fill="#1e293b">• cualquier contenido que un lector deba anunciar</text>
</svg>
```
