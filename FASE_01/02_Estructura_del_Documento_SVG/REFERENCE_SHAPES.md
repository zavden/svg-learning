# Referencia visual — Sección 02: Estructura del Documento SVG

---

## Anatomía completa de un SVG

```svg
<svg width="400" height="280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#0f172a;display:block; font-family:monospace;">

  <!-- Prólogo (solo .svg externo) -->
  <rect x="8" y="8" width="384" height="22" rx="4" fill="#1a2744"/>
  <text x="16" y="23" font-size="8" fill="#56b6c2">&lt;?xml version="1.0" encoding="UTF-8"?&gt;</text>
  <text x="316" y="23" font-size="7" fill="#5c6370" font-style="italic">solo en .svg</text>

  <!-- Elemento raíz svg -->
  <rect x="8" y="36" width="384" height="60" rx="4" fill="#0d2137"/>
  <text x="16" y="52" font-size="8" fill="#e06c75">&lt;svg</text>
  <text x="48" y="52" font-size="8" fill="#d19a66">xmlns</text>
  <text x="80" y="52" font-size="8" fill="#98c379">="http://www.w3.org/2000/svg"</text>
  <text x="48" y="64" font-size="8" fill="#d19a66">viewBox</text>
  <text x="88" y="64" font-size="8" fill="#98c379">="0 0 200 100"</text>
  <text x="176" y="64" font-size="8" fill="#d19a66">width</text>
  <text x="206" y="64" font-size="8" fill="#98c379">="200"</text>
  <text x="242" y="64" font-size="8" fill="#d19a66">height</text>
  <text x="278" y="64" font-size="8" fill="#98c379">="100"</text>
  <text x="48" y="76" font-size="8" fill="#d19a66">role</text>
  <text x="72" y="76" font-size="8" fill="#98c379">="img"</text>
  <text x="112" y="76" font-size="8" fill="#d19a66">aria-labelledby</text>
  <text x="196" y="76" font-size="8" fill="#98c379">="titulo desc"</text>
  <text x="278" y="76" font-size="8" fill="#56b6c2">&gt;</text>
  <text x="360" y="64" font-size="7" fill="#f59e0b">← raíz</text>

  <!-- title -->
  <rect x="20" y="102" width="360" height="18" rx="3" fill="#0d3b2d"/>
  <text x="28" y="115" font-size="8" fill="#e06c75">&lt;title</text>
  <text x="64" y="115" font-size="8" fill="#d19a66">id</text>
  <text x="76" y="115" font-size="8" fill="#98c379">="titulo"</text>
  <text x="122" y="115" font-size="8" fill="#56b6c2">&gt;</text>
  <text x="132" y="115" font-size="8" fill="#abb2bf">Nombre descriptivo del gráfico</text>
  <text x="304" y="115" font-size="8" fill="#e06c75">&lt;/title&gt;</text>
  <text x="358" y="115" font-size="7" fill="#10b981">← A11y</text>

  <!-- desc -->
  <rect x="20" y="124" width="360" height="18" rx="3" fill="#0d3b2d"/>
  <text x="28" y="137" font-size="8" fill="#e06c75">&lt;desc</text>
  <text x="60" y="137" font-size="8" fill="#d19a66">id</text>
  <text x="72" y="137" font-size="8" fill="#98c379">="desc"</text>
  <text x="114" y="137" font-size="8" fill="#56b6c2">&gt;</text>
  <text x="124" y="137" font-size="8" fill="#abb2bf">Descripción larga para AT</text>
  <text x="268" y="137" font-size="8" fill="#e06c75">&lt;/desc&gt;</text>
  <text x="358" y="137" font-size="7" fill="#10b981">← A11y</text>

  <!-- defs -->
  <rect x="20" y="148" width="360" height="46" rx="3" fill="#1e1a0d"/>
  <text x="28" y="163" font-size="8" fill="#e06c75">&lt;defs&gt;</text>
  <text x="28" y="180" font-size="8" fill="#5c6370" font-style="italic">
    &lt;linearGradient id="..."&gt; · &lt;filter&gt; · &lt;clipPath&gt; · &lt;symbol&gt; ···
  </text>
  <text x="28" y="192" font-size="8" fill="#e06c75">&lt;/defs&gt;</text>
  <text x="358" y="175" font-size="7" fill="#d19a66">← reutiliz.</text>

  <!-- contenido -->
  <rect x="20" y="200" width="360" height="28" rx="3" fill="#0d1a37"/>
  <text x="28" y="216" font-size="8" fill="#5c6370" font-style="italic">
    &lt;!-- Contenido gráfico: circle, rect, path, text, g... --&gt;
  </text>
  <text x="28" y="226" font-size="8" fill="#5c6370" font-style="italic">
    &lt;!-- Se renderiza en orden: primero = fondo, último = frente --&gt;
  </text>

  <!-- Cierre -->
  <rect x="8" y="234" width="384" height="18" rx="4" fill="#0d2137"/>
  <text x="16" y="247" font-size="8" fill="#e06c75">&lt;/svg&gt;</text>

  <!-- Leyenda -->
  <text x="200" y="268" text-anchor="middle"
        font-size="7" fill="#475569">
    Estructura completa de un SVG accesible para producción
  </text>
</svg>
```

---

## Modelo del pintor — orden de capas

```svg
<svg width="340" height="180"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <text x="170" y="18" text-anchor="middle"
        font-size="10" fill="#64748b" font-weight="600">
    Painter's Model — primero en código = más al fondo
  </text>

  <!-- Diagrama en perspectiva simulada -->
  <!-- Capa 4 (más al fondo) -->
  <rect x="20"  y="30" width="260" height="28" rx="4" fill="#1e3a8a"/>
  <text x="36" y="49" font-size="9" fill="#93c5fd">① &lt;rect fill="#1e3a8a"&gt;  ← primer elemento = FONDO</text>

  <!-- Capa 3 -->
  <rect x="28"  y="62" width="260" height="28" rx="4" fill="#1d4ed8"/>
  <text x="44" y="81" font-size="9" fill="#bfdbfe">② &lt;circle fill="#1d4ed8"&gt;</text>

  <!-- Capa 2 -->
  <rect x="36"  y="94" width="260" height="28" rx="4" fill="#3b82f6"/>
  <text x="52" y="113" font-size="9" fill="#dbeafe">③ &lt;path fill="#3b82f6"&gt;</text>

  <!-- Capa 1 (más al frente) -->
  <rect x="44"  y="126" width="260" height="28" rx="4" fill="#60a5fa"/>
  <text x="60" y="145" font-size="9" fill="white">④ &lt;text&gt;  ← último elemento = FRENTE</text>

  <text x="170" y="170" text-anchor="middle"
        font-size="8" fill="#94a3b8">
    No hay z-index en SVG 1.1 — el orden en el DOM es el orden visual
  </text>
</svg>
```

---

## Dónde va cada elemento en la estructura

```svg
<svg width="340" height="200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Cabecera -->
  <rect width="340" height="28" fill="#1e293b"/>
  <text x="85" y="19" text-anchor="middle" font-size="9" fill="#f1f5f9" font-weight="600">Elemento</text>
  <text x="200" y="19" text-anchor="middle" font-size="9" fill="#f1f5f9" font-weight="600">Dónde va</text>
  <text x="295" y="19" text-anchor="middle" font-size="9" fill="#f1f5f9" font-weight="600">Se renderiza</text>
  <line x1="160" y1="0" x2="160" y2="200" stroke="#334155" stroke-width="1"/>
  <line x1="258" y1="0" x2="258" y2="200" stroke="#334155" stroke-width="1"/>

  <rect x="0" y="28" width="340" height="24" fill="#eff6ff"/>
  <text x="85" y="45" text-anchor="middle" font-size="8" fill="#1e293b" font-family="monospace">&lt;title&gt;</text>
  <text x="200" y="45" text-anchor="middle" font-size="8" fill="#64748b">primer hijo de svg/g</text>
  <text x="295" y="45" text-anchor="middle" font-size="8" fill="#ef4444">No (solo lectores)</text>

  <rect x="0" y="52" width="340" height="24" fill="#f8fafc"/>
  <text x="85" y="69" text-anchor="middle" font-size="8" fill="#1e293b" font-family="monospace">&lt;desc&gt;</text>
  <text x="200" y="69" text-anchor="middle" font-size="8" fill="#64748b">tras &lt;title&gt;</text>
  <text x="295" y="69" text-anchor="middle" font-size="8" fill="#ef4444">No (solo lectores)</text>

  <rect x="0" y="76" width="340" height="24" fill="#eff6ff"/>
  <text x="85" y="93" text-anchor="middle" font-size="8" fill="#1e293b" font-family="monospace">&lt;defs&gt;</text>
  <text x="200" y="93" text-anchor="middle" font-size="8" fill="#64748b">antes del contenido</text>
  <text x="295" y="93" text-anchor="middle" font-size="8" fill="#ef4444">No (hasta referencia)</text>

  <rect x="0" y="100" width="340" height="24" fill="#f8fafc"/>
  <text x="85" y="117" text-anchor="middle" font-size="8" fill="#1e293b" font-family="monospace">linearGradient</text>
  <text x="200" y="117" text-anchor="middle" font-size="8" fill="#64748b">dentro de &lt;defs&gt;</text>
  <text x="295" y="117" text-anchor="middle" font-size="8" fill="#ef4444">No (solo via url(#))</text>

  <rect x="0" y="124" width="340" height="24" fill="#eff6ff"/>
  <text x="85" y="141" text-anchor="middle" font-size="8" fill="#1e293b" font-family="monospace">circle, rect, path</text>
  <text x="200" y="141" text-anchor="middle" font-size="8" fill="#64748b">contenido del svg</text>
  <text x="295" y="141" text-anchor="middle" font-size="8" fill="#10b981">Sí</text>

  <rect x="0" y="148" width="340" height="24" fill="#f8fafc"/>
  <text x="85" y="165" text-anchor="middle" font-size="8" fill="#1e293b" font-family="monospace">&lt;g&gt;</text>
  <text x="200" y="165" text-anchor="middle" font-size="8" fill="#64748b">contenido del svg</text>
  <text x="295" y="165" text-anchor="middle" font-size="8" fill="#10b981">Sí (agrupa)</text>

  <rect x="0" y="172" width="340" height="24" fill="#eff6ff"/>
  <text x="85" y="189" text-anchor="middle" font-size="8" fill="#1e293b" font-family="monospace">&lt;use&gt;</text>
  <text x="200" y="189" text-anchor="middle" font-size="8" fill="#64748b">contenido del svg</text>
  <text x="295" y="189" text-anchor="middle" font-size="8" fill="#10b981">Sí (instancia)</text>
</svg>
```
