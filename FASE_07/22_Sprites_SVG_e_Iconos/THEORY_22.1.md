# 22.1 Sistema de iconos con SVG: por qué SVG gana

Antes de SVG, había dos opciones para iconos: **fuentes de iconos** (FontAwesome, Material Icons como ttf) y **sprites PNG**. Ambas tienen limitaciones serias. SVG es hoy el estándar de facto y esta subsección explica por qué: resolución, color, accesibilidad y tamaño.

---

## Frente a icon fonts

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    SVG vs icon fonts
  </text>

  <rect x="10" y="32" width="280" height="20" fill="#e2e8f0"/>
  <text x="20" y="46" font-size="7" font-weight="bold" fill="#1e293b">Aspecto</text>
  <text x="150" y="46" font-size="7" font-weight="bold" fill="#1e293b">Icon fonts</text>
  <text x="230" y="46" font-size="7" font-weight="bold" fill="#1e293b">SVG</text>

  <rect x="10" y="52" width="280" height="22" fill="white"/>
  <text x="20" y="66" font-size="7" fill="#475569">colores múltiples</text>
  <text x="150" y="66" font-size="7" fill="#ef4444">✗ uno solo</text>
  <text x="230" y="66" font-size="7" fill="#10b981">✓</text>

  <rect x="10" y="74" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="88" font-size="7" fill="#475569">precisión visual</text>
  <text x="150" y="88" font-size="7" fill="#f59e0b">depende del font render</text>
  <text x="230" y="88" font-size="7" fill="#10b981">exacto</text>

  <rect x="10" y="96" width="280" height="22" fill="white"/>
  <text x="20" y="110" font-size="7" fill="#475569">alineación</text>
  <text x="150" y="110" font-size="7" fill="#ef4444">baseline, line-height</text>
  <text x="230" y="110" font-size="7" fill="#10b981">control total</text>

  <rect x="10" y="118" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="132" font-size="7" fill="#475569">accesibilidad</text>
  <text x="150" y="132" font-size="7" fill="#ef4444">problemático</text>
  <text x="230" y="132" font-size="7" fill="#10b981">title/aria nativo</text>

  <rect x="10" y="140" width="280" height="22" fill="white"/>
  <text x="20" y="154" font-size="7" fill="#475569">subset</text>
  <text x="150" y="154" font-size="7" fill="#f59e0b">build tools</text>
  <text x="230" y="154" font-size="7" fill="#10b981">trivial</text>

  <rect x="10" y="162" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="176" font-size="7" fill="#475569">fallo de carga</text>
  <text x="150" y="176" font-size="7" fill="#ef4444">cuadros raros</text>
  <text x="230" y="176" font-size="7" fill="#10b981">fallback controlable</text>

  <rect x="10" y="184" width="280" height="22" fill="white"/>
  <text x="20" y="198" font-size="7" fill="#475569">animación</text>
  <text x="150" y="198" font-size="7" fill="#ef4444">no</text>
  <text x="230" y="198" font-size="7" fill="#10b981">sí</text>

  <text x="150" y="228" text-anchor="middle" font-size="7" fill="#94a3b8">las icon fonts tratan cada icono como un glifo tipográfico</text>
  <text x="150" y="242" text-anchor="middle" font-size="7" fill="#94a3b8">→ heredan TODOS los problemas de renderizado de fuentes</text>
  <text x="150" y="256" text-anchor="middle" font-size="7" fill="#94a3b8">SVG es un gráfico real, no un "carácter especial"</text>
</svg>
```

---

## Frente a imágenes raster

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    SVG vs PNG/WebP
  </text>

  <rect x="10" y="32" width="280" height="20" fill="#e2e8f0"/>
  <text x="20" y="46" font-size="7" font-weight="bold" fill="#1e293b">Aspecto</text>
  <text x="150" y="46" font-size="7" font-weight="bold" fill="#1e293b">PNG/WebP</text>
  <text x="230" y="46" font-size="7" font-weight="bold" fill="#1e293b">SVG</text>

  <rect x="10" y="52" width="280" height="22" fill="white"/>
  <text x="20" y="66" font-size="7" fill="#475569">pantallas HiDPI</text>
  <text x="150" y="66" font-size="7" fill="#ef4444">requiere @2x, @3x</text>
  <text x="230" y="66" font-size="7" fill="#10b981">una sola versión</text>

  <rect x="10" y="74" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="88" font-size="7" fill="#475569">cambio de color</text>
  <text x="150" y="88" font-size="7" fill="#ef4444">imposible en CSS</text>
  <text x="230" y="88" font-size="7" fill="#10b981">currentColor</text>

  <rect x="10" y="96" width="280" height="22" fill="white"/>
  <text x="20" y="110" font-size="7" fill="#475569">peso (iconos simples)</text>
  <text x="150" y="110" font-size="7" fill="#f59e0b">variable</text>
  <text x="230" y="110" font-size="7" fill="#10b981">generalmente menor</text>

  <rect x="10" y="118" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="132" font-size="7" fill="#475569">animación</text>
  <text x="150" y="132" font-size="7" fill="#ef4444">no</text>
  <text x="230" y="132" font-size="7" fill="#10b981">sí</text>

  <rect x="10" y="140" width="280" height="22" fill="white"/>
  <text x="20" y="154" font-size="7" fill="#475569">interactividad</text>
  <text x="150" y="154" font-size="7" fill="#ef4444">no</text>
  <text x="230" y="154" font-size="7" fill="#10b981">sí</text>

  <rect x="10" y="162" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="176" font-size="7" fill="#475569">tematización</text>
  <text x="150" y="176" font-size="7" fill="#ef4444">duplicar archivos</text>
  <text x="230" y="176" font-size="7" fill="#10b981">variables CSS</text>

  <rect x="15" y="198" width="270" height="50" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="214" font-size="8" font-weight="bold" fill="#92400e">Única excepción</text>
  <text x="25" y="228" font-size="7" fill="#475569">iconos muy complejos (fotográficos, degradados sutiles)</text>
  <text x="25" y="240" font-size="7" fill="#475569">donde un PNG optimizado sale más barato en KB</text>
</svg>
```

---

## El argumento del `currentColor`

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    la "magia" del color heredado
  </text>

  <rect x="15" y="38" width="270" height="82" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#166534">La idea</text>
  <text x="25" y="70" font-size="7" fill="#475569">el icono declara fill="currentColor" una sola vez</text>
  <text x="25" y="84" font-size="7" fill="#475569">y luego CADA botón, enlace o texto que lo contenga</text>
  <text x="25" y="98" font-size="7" fill="#475569">lo tiñe con su propio color sin tocar el SVG</text>
  <text x="25" y="114" font-size="7" fill="#166534">un solo icono → todos los colores del tema</text>

  <rect x="15" y="128" width="270" height="100" fill="#0f172a" rx="3"/>
  <text x="25" y="146" font-size="8" font-family="monospace" fill="#60a5fa">&lt;symbol id="check" viewBox="0 0 24 24"&gt;</text>
  <text x="25" y="160" font-size="8" font-family="monospace" fill="#34d399">  &lt;path fill="currentColor" d="..."/&gt;</text>
  <text x="25" y="174" font-size="8" font-family="monospace" fill="#60a5fa">&lt;/symbol&gt;</text>

  <text x="25" y="196" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;button style="color: red"&gt;</text>
  <text x="25" y="210" font-size="8" font-family="monospace" fill="#fbbf24">  &lt;svg&gt;&lt;use href="#check"/&gt;&lt;/svg&gt;</text>
  <text x="25" y="224" font-size="7" fill="#94a3b8">// el check se renderiza en rojo automáticamente</text>
</svg>
```

---

## Conclusión

```svg
<svg width="300" height="180"
     viewBox="0 0 300 180"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    el veredicto
  </text>

  <rect x="15" y="38" width="270" height="130" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="58" font-size="10" font-weight="bold" fill="#166534">SVG es el formato correcto</text>
  <text x="25" y="72" font-size="7" fill="#475569">para sistemas de iconos en la web moderna</text>

  <text x="25" y="96" font-size="8" fill="#166534">✓ calidad en cualquier resolución</text>
  <text x="25" y="110" font-size="8" fill="#166534">✓ color dinámico sin duplicar archivos</text>
  <text x="25" y="124" font-size="8" fill="#166534">✓ accesibilidad real (aria, title)</text>
  <text x="25" y="138" font-size="8" fill="#166534">✓ animable e interactivo</text>
  <text x="25" y="152" font-size="8" fill="#166534">✓ todo el ecosistema moderno (Material, Heroicons, Lucide…)</text>
</svg>
```
