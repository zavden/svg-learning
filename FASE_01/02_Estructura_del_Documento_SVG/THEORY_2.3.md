# 2.3 Elementos de metadatos

SVG tiene tres elementos de metadatos: `<title>`, `<desc>` y `<metadata>`. Los dos primeros son fundamentales para la accesibilidad.

---

## `<title>` — el texto alternativo del SVG

`<title>` es al SVG lo que el atributo `alt` es a `<img>`: el texto que describe el gráfico para quien no puede verlo. Los lectores de pantalla lo anuncian; el navegador lo muestra como tooltip.

```svg
<svg width="300" height="140"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- ✓ BIEN: <title> como primer hijo del <svg> -->
  <rect x="10" y="10" width="280" height="120" rx="8"
        fill="#eff6ff" stroke="#3b82f6" stroke-width="1.5"/>

  <!-- En este ejemplo, el <title> está en el código SVG renderizado -->
  <text x="150" y="35" text-anchor="middle"
        font-size="10" fill="#1e40af" font-weight="600">
    Gráfico de ventas Q4 2024
  </text>

  <text x="150" y="55" text-anchor="middle"
        font-size="8" fill="#64748b">
    ↑ Este sería el contenido del &lt;title&gt;
  </text>

  <!-- Simulación de tooltip -->
  <rect x="60" y="68" width="180" height="30" rx="6"
        fill="#1e293b" opacity="0.9"/>
  <text x="150" y="83" text-anchor="middle"
        font-size="8" fill="#f1f5f9">
    Tooltip del navegador: muestra el &lt;title&gt;
  </text>
  <text x="150" y="95" text-anchor="middle"
        font-size="8" fill="#94a3b8">
    "Gráfico de ventas Q4 2024"
  </text>

  <!-- Flecha de tooltip -->
  <polygon points="150,108 140,100 160,100" fill="#1e293b" opacity="0.9"/>

  <!-- Ícono de lector de pantalla -->
  <text x="150" y="128" text-anchor="middle"
        font-size="8" fill="#64748b">
    Lector de pantalla anuncia este texto al usuario
  </text>
</svg>
```

---

## Posición correcta del `<title>`

`<title>` debe ser el **primer hijo** del elemento que describe. Si describe el SVG completo, va como primer hijo de `<svg>`. Si describe un grupo, va como primer hijo de `<g>`.

```svg
<svg width="300" height="150"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#0f172a;display:block; font-family:monospace;">

  <!-- CORRECTO -->
  <text x="16" y="22" font-size="9" fill="#10b981">✓ Correcto:</text>

  <text x="16" y="40" font-size="9" fill="#e06c75">&lt;svg</text>
  <text x="50" y="40" font-size="9" fill="#d19a66">role</text>
  <text x="76" y="40" font-size="9" fill="#56b6c2">=</text>
  <text x="84" y="40" font-size="9" fill="#98c379">"img"</text>
  <text x="118" y="40" font-size="9" fill="#d19a66">aria-labelledby</text>
  <text x="200" y="40" font-size="9" fill="#56b6c2">=</text>
  <text x="208" y="40" font-size="9" fill="#98c379">"t1"</text>
  <text x="232" y="40" font-size="9" fill="#56b6c2">&gt;</text>

  <text x="28" y="56" font-size="9" fill="#e06c75">  &lt;title</text>
  <text x="76" y="56" font-size="9" fill="#d19a66">id</text>
  <text x="90" y="56" font-size="9" fill="#56b6c2">=</text>
  <text x="98" y="56" font-size="9" fill="#98c379">"t1"</text>
  <text x="126" y="56" font-size="9" fill="#56b6c2">&gt;</text>
  <text x="142" y="56" font-size="9" fill="#abb2bf">Ventas 2024</text>
  <text x="222" y="56" font-size="9" fill="#e06c75">&lt;/title&gt;</text>

  <text x="28" y="72" font-size="9" fill="#5c6370" font-style="italic">
    &lt;!-- contenido gráfico --&gt;
  </text>
  <text x="16" y="88" font-size="9" fill="#e06c75">&lt;/svg&gt;</text>

  <!-- INCORRECTO -->
  <text x="16" y="108" font-size="9" fill="#ef4444">✗ Incorrecto (title al final):</text>

  <text x="16" y="124" font-size="9" fill="#5c6370" font-style="italic">
    &lt;!-- contenido gráfico --&gt;
  </text>
  <text x="16" y="140" font-size="9" fill="#f87171">
    &lt;title&gt;...&lt;/title&gt;  &lt;!-- tarde: algunos AT no lo leen --&gt;
  </text>
</svg>
```

---

## `<desc>` — descripción larga

Mientras `<title>` es breve (como un `alt`), `<desc>` puede contener una descripción detallada del contenido. Es para gráficos complejos donde el título no es suficiente.

```svg
<svg width="300" height="160"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Simulación de un gráfico con title + desc -->

  <!-- Gráfico de barras simplificado -->
  <rect x="20" y="20" width="260" height="100" rx="6" fill="#f1f5f9"/>

  <!-- Barras -->
  <rect x="40"  y="70" width="30" height="50" rx="3" fill="#3b82f6"/>
  <rect x="90"  y="45" width="30" height="75" rx="3" fill="#3b82f6"/>
  <rect x="140" y="60" width="30" height="60" rx="3" fill="#10b981"/>
  <rect x="190" y="35" width="30" height="85" rx="3" fill="#10b981"/>
  <rect x="240" y="55" width="30" height="65" rx="3" fill="#3b82f6"/>

  <!-- Labels -->
  <text x="55"  y="132" text-anchor="middle" font-size="7" fill="#64748b">Q1</text>
  <text x="105" y="132" text-anchor="middle" font-size="7" fill="#64748b">Q2</text>
  <text x="155" y="132" text-anchor="middle" font-size="7" fill="#64748b">Q3</text>
  <text x="205" y="132" text-anchor="middle" font-size="7" fill="#64748b">Q4</text>
  <text x="255" y="132" text-anchor="middle" font-size="7" fill="#64748b">Q5</text>

  <!-- Anotación del title y desc -->
  <text x="150" y="148" text-anchor="middle"
        font-size="8" fill="#64748b">
    &lt;title&gt;: "Ventas por trimestre 2024"
  </text>
  <text x="150" y="160" text-anchor="middle"
        font-size="7" fill="#94a3b8">
    &lt;desc&gt;: "El mayor crecimiento fue en Q4 con 85K de ventas..."
  </text>
</svg>
```

---

## Patrones de accesibilidad SVG

Tres situaciones distintas requieren atributos ARIA distintos:

```svg
<svg width="300" height="200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#0f172a;display:block; font-family:monospace;">

  <!-- Caso 1: Decorativo (sin significado) -->
  <rect x="8" y="8" width="284" height="54" rx="5" fill="#0d2137"/>
  <text x="150" y="25" text-anchor="middle" font-size="8" fill="#6a9955">
    Caso 1: SVG decorativo (sin info)
  </text>
  <text x="16" y="42" font-size="8" fill="#abb2bf">&lt;svg</text>
  <text x="44" y="42" font-size="8" fill="#d19a66">aria-hidden</text>
  <text x="106" y="42" font-size="8" fill="#56b6c2">=</text>
  <text x="114" y="42" font-size="8" fill="#98c379">"true"</text>
  <text x="150" y="42" font-size="8" fill="#d19a66">focusable</text>
  <text x="200" y="42" font-size="8" fill="#56b6c2">=</text>
  <text x="208" y="42" font-size="8" fill="#98c379">"false"</text>
  <text x="254" y="42" font-size="8" fill="#56b6c2">&gt;</text>

  <!-- Caso 2: Icono con label simple -->
  <rect x="8" y="68" width="284" height="54" rx="5" fill="#0d2137"/>
  <text x="150" y="84" text-anchor="middle" font-size="8" fill="#6a9955">
    Caso 2: Icono con significado propio
  </text>
  <text x="16" y="100" font-size="8" fill="#abb2bf">&lt;svg</text>
  <text x="44" y="100" font-size="8" fill="#d19a66">role</text>
  <text x="72" y="100" font-size="8" fill="#56b6c2">=</text>
  <text x="80" y="100" font-size="8" fill="#98c379">"img"</text>
  <text x="114" y="100" font-size="8" fill="#d19a66">aria-label</text>
  <text x="166" y="100" font-size="8" fill="#56b6c2">=</text>
  <text x="174" y="100" font-size="8" fill="#98c379">"Cerrar"</text>
  <text x="218" y="100" font-size="8" fill="#56b6c2">&gt;</text>

  <!-- Caso 3: Gráfico complejo con title+desc -->
  <rect x="8" y="128" width="284" height="64" rx="5" fill="#0d2137"/>
  <text x="150" y="144" text-anchor="middle" font-size="8" fill="#6a9955">
    Caso 3: Gráfico complejo
  </text>
  <text x="16" y="160" font-size="8" fill="#abb2bf">&lt;svg</text>
  <text x="44" y="160" font-size="8" fill="#d19a66">role</text>
  <text x="72" y="160" font-size="8" fill="#56b6c2">=</text>
  <text x="80" y="160" font-size="8" fill="#98c379">"img"</text>
  <text x="114" y="160" font-size="8" fill="#d19a66">aria-labelledby</text>
  <text x="198" y="160" font-size="8" fill="#56b6c2">=</text>
  <text x="206" y="160" font-size="8" fill="#98c379">"t d"</text>
  <text x="234" y="160" font-size="8" fill="#56b6c2">&gt;</text>
  <text x="28" y="180" font-size="8" fill="#e06c75">  &lt;title</text>
  <text x="72" y="180" font-size="8" fill="#d19a66">id</text>
  <text x="84" y="180" font-size="8" fill="#56b6c2">=</text>
  <text x="92" y="180" font-size="8" fill="#98c379">"t"</text>
  <text x="114" y="180" font-size="8" fill="#56b6c2">&gt;</text>
  <text x="128" y="180" font-size="8" fill="#abb2bf">Ventas Q4</text>
  <text x="190" y="180" font-size="8" fill="#e06c75">&lt;/title&gt;</text>
  <text x="228" y="180" font-size="8" fill="#e06c75">  &lt;desc</text>
  <text x="262" y="180" font-size="8" fill="#d19a66">id</text>
  <text x="274" y="180" font-size="8" fill="#56b6c2">=</text>
  <text x="282" y="180" font-size="8" fill="#98c379">"d"</text>
</svg>
```

---

## `<metadata>` — metadatos estructurados

`<metadata>` es el contenedor para datos en formatos externos como RDF o Dublin Core. Lo generan los editores gráficos (Inkscape). Para SVG en web, suele eliminarse con SVGO.

```svg
<svg width="300" height="120"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <text x="150" y="20" text-anchor="middle"
        font-size="10" fill="#64748b">¿Cuándo importa cada elemento?</text>

  <!-- Tabla -->
  <rect x="10" y="28" width="280" height="82" rx="6"
        fill="#f1f5f9" stroke="#e2e8f0" stroke-width="1"/>

  <text x="20" y="46" font-size="9" fill="#1e293b" font-weight="600" font-family="monospace">&lt;title&gt;</text>
  <text x="80" y="46" font-size="8" fill="#10b981">Siempre (accesibilidad)</text>

  <text x="20" y="62" font-size="9" fill="#1e293b" font-weight="600" font-family="monospace">&lt;desc&gt;</text>
  <text x="80" y="62" font-size="8" fill="#f59e0b">SVGs complejos con datos</text>

  <text x="20" y="78" font-size="9" fill="#1e293b" font-weight="600" font-family="monospace">&lt;metadata&gt;</text>
  <text x="80" y="78" font-size="8" fill="#ef4444">Solo editores/herramientas</text>
  <text x="80" y="90" font-size="8" fill="#94a3b8">(eliminar en producción web)</text>

  <line x1="10" y1="98" x2="290" y2="98" stroke="#e2e8f0" stroke-width="1"/>
  <text x="150" y="110" text-anchor="middle"
        font-size="8" fill="#94a3b8">
    &lt;title&gt; y &lt;desc&gt; son visibles para AT · &lt;metadata&gt; no
  </text>
</svg>
```
