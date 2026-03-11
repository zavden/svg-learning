# 2.2 Prólogo XML

El prólogo XML es la declaración opcional que aparece antes del elemento raíz. Indica la versión de XML y la codificación del archivo.

---

## La declaración XML

```svg
<svg width="340" height="110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#0f172a;display:block; font-family:monospace;">

  <!-- La declaración XML completa -->
  <text x="16" y="30" font-size="9" fill="#56b6c2">&lt;?xml</text>
  <text x="60" y="30" font-size="9" fill="#d19a66">version</text>
  <text x="108" y="30" font-size="9" fill="#56b6c2">=</text>
  <text x="116" y="30" font-size="9" fill="#98c379">"1.0"</text>
  <text x="154" y="30" font-size="9" fill="#d19a66">encoding</text>
  <text x="208" y="30" font-size="9" fill="#56b6c2">=</text>
  <text x="216" y="30" font-size="9" fill="#98c379">"UTF-8"</text>
  <text x="264" y="30" font-size="9" fill="#56b6c2">?&gt;</text>

  <!-- Anotaciones -->
  <line x1="40" y1="36" x2="40" y2="50" stroke="#475569" stroke-width="1"/>
  <text x="40" y="62" text-anchor="middle" font-size="7" fill="#475569">instrucción</text>
  <text x="40" y="72" text-anchor="middle" font-size="7" fill="#475569">de proceso</text>

  <line x1="130" y1="36" x2="130" y2="50" stroke="#475569" stroke-width="1"/>
  <text x="130" y="62" text-anchor="middle" font-size="7" fill="#475569">siempre</text>
  <text x="130" y="72" text-anchor="middle" font-size="7" fill="#475569">"1.0"</text>

  <line x1="236" y1="36" x2="236" y2="50" stroke="#475569" stroke-width="1"/>
  <text x="236" y="62" text-anchor="middle" font-size="7" fill="#475569">codificación</text>
  <text x="236" y="72" text-anchor="middle" font-size="7" fill="#475569">del archivo</text>

  <!-- Advertencia -->
  <rect x="10" y="82" width="320" height="22" rx="4" fill="#422006" opacity="0.6"/>
  <text x="170" y="97" text-anchor="middle" font-size="8" fill="#fde68a">
    ⚠ Debe ser la primera línea del archivo — sin nada antes (ni espacio, ni BOM)
  </text>
</svg>
```

---

## Cuándo incluirlo y cuándo no

La regla es simple: solo en archivos `.svg` independientes, nunca en SVG inline dentro de HTML.

```svg
<svg width="300" height="160"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Columna: .svg externo -->
  <rect x="10" y="10" width="130" height="140" rx="8"
        fill="#f0fdf4" stroke="#10b981" stroke-width="1.5"/>
  <text x="75" y="30" text-anchor="middle"
        font-size="10" fill="#166534" font-weight="600">archivo.svg</text>

  <rect x="20" y="40" width="110" height="36" rx="4" fill="#0f172a"/>
  <text x="75" y="55" text-anchor="middle" font-size="8" fill="#56b6c2" font-family="monospace">
    &lt;?xml version="1.0"
  </text>
  <text x="75" y="69" text-anchor="middle" font-size="8" fill="#56b6c2" font-family="monospace">
     encoding="UTF-8"?&gt;
  </text>

  <text x="75" y="95" text-anchor="middle" font-size="8" fill="#166534">✓ Recomendado</text>
  <text x="75" y="109" text-anchor="middle" font-size="8" fill="#64748b">Ayuda a parsers</text>
  <text x="75" y="122" text-anchor="middle" font-size="8" fill="#64748b">y editores XML</text>
  <text x="75" y="138" text-anchor="middle" font-size="8" fill="#64748b">a leer el archivo</text>

  <!-- Columna: inline HTML -->
  <rect x="160" y="10" width="130" height="140" rx="8"
        fill="#fef2f2" stroke="#ef4444" stroke-width="1.5"/>
  <text x="225" y="30" text-anchor="middle"
        font-size="10" fill="#991b1b" font-weight="600">SVG inline en HTML</text>

  <rect x="170" y="40" width="110" height="36" rx="4" fill="#0f172a"/>
  <text x="225" y="55" text-anchor="middle" font-size="8" fill="#f87171" font-family="monospace">
    &lt;?xml version="1.0"
  </text>
  <text x="225" y="69" text-anchor="middle" font-size="8" fill="#f87171" font-family="monospace">
    encoding="UTF-8"?&gt;
  </text>

  <text x="225" y="95" text-anchor="middle" font-size="8" fill="#991b1b">✗ No incluir</text>
  <text x="225" y="109" text-anchor="middle" font-size="8" fill="#64748b">El parser HTML</text>
  <text x="225" y="122" text-anchor="middle" font-size="8" fill="#64748b">no espera una</text>
  <text x="225" y="138" text-anchor="middle" font-size="8" fill="#64748b">instrucción XML aquí</text>
</svg>
```

---

## UTF-8 y caracteres especiales

Declarar `encoding="UTF-8"` indica que el archivo usa la codificación Unicode estándar. Esto es especialmente importante cuando el SVG contiene texto con caracteres no-ASCII.

```svg
<svg width="300" height="130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <text x="150" y="20" text-anchor="middle"
        font-size="10" fill="#64748b">
    UTF-8 soporta cualquier carácter Unicode
  </text>

  <!-- Con UTF-8 estos funcionan sin problemas -->
  <text x="40" y="55" font-size="22" fill="#3b82f6">á é í ó ú</text>
  <text x="40" y="80" font-size="22" fill="#10b981">ñ ü ç ß ø</text>
  <text x="40" y="105" font-size="22" fill="#f59e0b">中文 日本語 한글</text>

  <text x="150" y="122" text-anchor="middle"
        font-size="8" fill="#94a3b8">
    Con encoding="UTF-8" en el prólogo — todos estos se renderizan correctamente
  </text>
</svg>
```

---

## DOCTYPE en SVG — por qué no usarlo

Algunos SVGs antiguos incluyen una declaración DOCTYPE. Los navegadores modernos la ignoran, pero puede generar una petición HTTP innecesaria.

```svg
<svg width="300" height="140"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#0f172a;display:block; font-family:monospace;">

  <!-- Lo que generaban los editores antiguos -->
  <text x="16" y="24" font-size="8" fill="#6a9955">
    &lt;!-- SVG antiguo (editores de los 2000s) --&gt;
  </text>
  <text x="16" y="42" font-size="8" fill="#5c6370">
    &lt;!DOCTYPE svg PUBLIC
  </text>
  <text x="16" y="56" font-size="8" fill="#5c6370">
    "-//W3C//DTD SVG 1.1//EN"
  </text>
  <text x="16" y="70" font-size="8" fill="#5c6370">
    "http://www.w3.org/Graphics/SVG/..."&gt;
  </text>

  <!-- Lo que hace hoy -->
  <rect x="10" y="80" width="280" height="50" rx="4" fill="#1e293b"/>
  <text x="150" y="100" text-anchor="middle" font-size="8" fill="#f59e0b">
    Hoy: los navegadores lo ignoran.
  </text>
  <text x="150" y="114" text-anchor="middle" font-size="8" fill="#64748b">
    Puede generar un request HTTP al W3C.
  </text>
  <text x="150" y="126" text-anchor="middle" font-size="8" fill="#ef4444">
    SVGO lo elimina por defecto. No incluirlo.
  </text>
</svg>
```
