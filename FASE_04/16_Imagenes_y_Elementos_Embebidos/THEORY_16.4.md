# 16.4 Imágenes base64 embebidas

Un **data URI** permite incrustar el contenido completo de una imagen dentro del propio atributo `href`, eliminando la necesidad de un archivo externo. El SVG queda **autosuficiente**: un solo archivo lleva todo.

---

## Anatomía de un data URI

```svg
<svg width="300" height="200"
     viewBox="0 0 300 200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">estructura de un data URI</text>

  <!-- Caja con el desglose -->
  <rect x="20" y="40" width="260" height="140" fill="white" stroke="#cbd5e1" rx="4"/>

  <!-- Línea completa -->
  <text x="30" y="60" font-size="9" font-family="monospace" fill="#1e293b">data:image/png;base64,iVBORw0KGgo...</text>

  <!-- Partes resaltadas -->
  <rect x="30" y="78" width="28" height="14" fill="#dbeafe" stroke="#3b82f6"/>
  <text x="44" y="89" text-anchor="middle" font-size="8" font-family="monospace" fill="#1e40af">data:</text>
  <text x="30" y="106" font-size="7" fill="#64748b">esquema</text>

  <rect x="62" y="78" width="58" height="14" fill="#dcfce7" stroke="#10b981"/>
  <text x="91" y="89" text-anchor="middle" font-size="8" font-family="monospace" fill="#166534">image/png</text>
  <text x="62" y="106" font-size="7" fill="#64748b">MIME type</text>

  <rect x="124" y="78" width="42" height="14" fill="#fef9c3" stroke="#f59e0b"/>
  <text x="145" y="89" text-anchor="middle" font-size="8" font-family="monospace" fill="#92400e">;base64</text>
  <text x="124" y="106" font-size="7" fill="#64748b">codificación</text>

  <rect x="170" y="78" width="12" height="14" fill="#fee2e2" stroke="#ef4444"/>
  <text x="176" y="89" text-anchor="middle" font-size="8" font-family="monospace" fill="#991b1b">,</text>
  <text x="168" y="106" font-size="7" fill="#64748b">separador</text>

  <rect x="186" y="78" width="90" height="14" fill="#ede9fe" stroke="#8b5cf6"/>
  <text x="231" y="89" text-anchor="middle" font-size="8" font-family="monospace" fill="#5b21b6">iVBORw0KGgo...</text>
  <text x="186" y="106" font-size="7" fill="#64748b">datos codificados</text>

  <!-- Notas -->
  <text x="30" y="130" font-size="7" fill="#475569">• MIME types comunes: image/png, image/jpeg, image/svg+xml, image/webp</text>
  <text x="30" y="144" font-size="7" fill="#475569">• base64 es la codificación más común; también existe ;utf8 o nada (URL-encoded)</text>
  <text x="30" y="158" font-size="7" fill="#475569">• el resultado es una cadena larga pero autosuficiente</text>
  <text x="30" y="172" font-size="7" fill="#475569">• se usa en href="..." de &lt;image&gt;, en src="..." de HTML y en CSS url(...)</text>
</svg>
```

---

## Un PNG real embebido (1×1 rojo escalado)

```svg
<svg width="300" height="200"
     viewBox="0 0 300 200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">PNG de 1×1 pixel rojo, embebido y escalado</text>

  <rect x="20" y="40" width="260" height="140" fill="white" stroke="#cbd5e1" rx="4"/>

  <!-- El PNG en sí: 1x1 píxel rojo. Escalado sin suavizado se ve como un bloque. -->
  <image x="40" y="60" width="80" height="80"
         preserveAspectRatio="none"
         style="image-rendering:pixelated"
         href="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR42mP8z8BQDwAEhQGAhKmMIQAAAABJRU5ErkJggg=="/>

  <text x="80" y="155" text-anchor="middle" font-size="7" fill="#64748b">1 píxel → bloque 80×80</text>

  <!-- Explicación al lado -->
  <text x="140" y="68" font-size="8" font-weight="bold" fill="#1e293b">el PNG completo:</text>
  <text x="140" y="82" font-size="6" font-family="monospace" fill="#475569">data:image/png;base64,</text>
  <text x="140" y="93" font-size="6" font-family="monospace" fill="#475569">iVBORw0KGgoAAAANSUhEUg</text>
  <text x="140" y="104" font-size="6" font-family="monospace" fill="#475569">AAAAEAAAABCAYAAAAfFcSJ</text>
  <text x="140" y="115" font-size="6" font-family="monospace" fill="#475569">AAAADUlEQVR42mP8z8BQDw</text>
  <text x="140" y="126" font-size="6" font-family="monospace" fill="#475569">AEhQGAhKmMIQAAAABJRU5</text>
  <text x="140" y="137" font-size="6" font-family="monospace" fill="#475569">ErkJggg==</text>

  <text x="140" y="155" font-size="6" fill="#10b981">↑ 98 caracteres para 1 píxel</text>
  <text x="140" y="166" font-size="6" fill="#64748b">una imagen real es mucho más larga</text>
</svg>
```

---

## SVG dentro de SVG mediante data URI

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">SVG embebido como data URI (formato URL-encoded)</text>

  <!-- Tres copias del mismo SVG embebido en distintos tamaños -->
  <rect x="20" y="40" width="260" height="150" fill="white" stroke="#cbd5e1" rx="4"/>

  <!-- Versión grande -->
  <image x="40" y="60" width="60" height="60"
         href="data:image/svg+xml;utf8,%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20viewBox%3D%220%200%20100%20100%22%3E%3Ccircle%20cx%3D%2250%22%20cy%3D%2250%22%20r%3D%2240%22%20fill%3D%22%233b82f6%22%2F%3E%3Cpath%20d%3D%22M%2035%2050%20L%2045%2060%20L%2065%2040%22%20stroke%3D%22white%22%20stroke-width%3D%228%22%20fill%3D%22none%22%20stroke-linecap%3D%22round%22%20stroke-linejoin%3D%22round%22%2F%3E%3C%2Fsvg%3E"/>
  <text x="70" y="135" text-anchor="middle" font-size="7" fill="#64748b">60×60</text>

  <!-- Versión mediana -->
  <image x="120" y="70" width="40" height="40"
         href="data:image/svg+xml;utf8,%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20viewBox%3D%220%200%20100%20100%22%3E%3Ccircle%20cx%3D%2250%22%20cy%3D%2250%22%20r%3D%2240%22%20fill%3D%22%233b82f6%22%2F%3E%3Cpath%20d%3D%22M%2035%2050%20L%2045%2060%20L%2065%2040%22%20stroke%3D%22white%22%20stroke-width%3D%228%22%20fill%3D%22none%22%20stroke-linecap%3D%22round%22%20stroke-linejoin%3D%22round%22%2F%3E%3C%2Fsvg%3E"/>
  <text x="140" y="125" text-anchor="middle" font-size="7" fill="#64748b">40×40</text>

  <!-- Versión pequeña -->
  <image x="180" y="80" width="20" height="20"
         href="data:image/svg+xml;utf8,%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20viewBox%3D%220%200%20100%20100%22%3E%3Ccircle%20cx%3D%2250%22%20cy%3D%2250%22%20r%3D%2240%22%20fill%3D%22%233b82f6%22%2F%3E%3Cpath%20d%3D%22M%2035%2050%20L%2045%2060%20L%2065%2040%22%20stroke%3D%22white%22%20stroke-width%3D%228%22%20fill%3D%22none%22%20stroke-linecap%3D%22round%22%20stroke-linejoin%3D%22round%22%2F%3E%3C%2Fsvg%3E"/>
  <text x="190" y="115" text-anchor="middle" font-size="7" fill="#64748b">20×20</text>

  <!-- Código simplificado -->
  <text x="215" y="75" font-size="7" font-family="monospace" fill="#475569">data:image/svg+xml;utf8,</text>
  <text x="215" y="87" font-size="7" font-family="monospace" fill="#475569">%3Csvg...%3E</text>
  <text x="215" y="99" font-size="7" font-family="monospace" fill="#475569">%3Ccircle.../%3E</text>
  <text x="215" y="111" font-size="7" font-family="monospace" fill="#475569">%3C/svg%3E</text>

  <text x="150" y="160" text-anchor="middle" font-size="7" fill="#64748b">
    cada &lt;image&gt; embebe el SVG completo codificado
  </text>
  <text x="150" y="172" text-anchor="middle" font-size="7" fill="#94a3b8">
    (para SVG, URL-encoded es más eficiente que base64)
  </text>
  <text x="150" y="183" text-anchor="middle" font-size="7" fill="#94a3b8">
    escala sin pérdida porque el contenido sigue siendo vectorial
  </text>
</svg>
```

---

## base64 vs URL-encoding

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">dos formas de codificar el mismo contenido</text>

  <!-- base64 -->
  <rect x="15" y="35" width="270" height="85" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="50" font-size="8" font-weight="bold" fill="#1e40af">base64</text>
  <text x="25" y="62" font-size="7" fill="#475569">data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53</text>
  <text x="25" y="73" font-size="7" fill="#475569">My5vcmcvMjAwMC9zdmciPjxjaXJjbGUgY3g9IjUiIGN5PSI1IiByPSI0Ii8+</text>
  <text x="25" y="84" font-size="7" fill="#475569">PC9zdmc+</text>
  <text x="25" y="100" font-size="7" fill="#10b981">✓ obligatorio para binarios (PNG/JPEG)</text>
  <text x="25" y="112" font-size="7" fill="#ef4444">✗ ~33% más grande que el original</text>

  <!-- URL-encoded -->
  <rect x="15" y="130" width="270" height="85" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="145" font-size="8" font-weight="bold" fill="#166534">URL-encoded (solo texto/SVG)</text>
  <text x="25" y="157" font-size="7" fill="#475569">data:image/svg+xml;utf8,%3Csvg%20xmlns%3D%22http%3A%2F%2F</text>
  <text x="25" y="168" font-size="7" fill="#475569">www.w3.org%2F2000%2Fsvg%22%3E%3Ccircle%20cx%3D%225%22</text>
  <text x="25" y="179" font-size="7" fill="#475569">%20cy%3D%225%22%20r%3D%224%22%2F%3E%3C%2Fsvg%3E</text>
  <text x="25" y="195" font-size="7" fill="#10b981">✓ más legible y compacto para SVG</text>
  <text x="25" y="207" font-size="7" fill="#10b981">✓ se puede inspeccionar y editar a mano</text>

  <text x="150" y="230" text-anchor="middle" font-size="7" fill="#94a3b8">
    para SVG elige URL-encoded; para PNG/JPEG usa base64
  </text>
</svg>
```

---

## Ventajas y desventajas

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    ¿cuándo embeber con base64?
  </text>

  <!-- Ventajas -->
  <rect x="10" y="36" width="280" height="100" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="18" y="52" font-size="9" font-weight="bold" fill="#166534">✓ Ventajas</text>
  <text x="18" y="67" font-size="7" fill="#1e293b">• SVG autosuficiente: un solo archivo, sin dependencias externas</text>
  <text x="18" y="80" font-size="7" fill="#1e293b">• sin problemas de CORS cuando se procesa en canvas</text>
  <text x="18" y="93" font-size="7" fill="#1e293b">• funciona offline y en &lt;img src="..."&gt; de SVG</text>
  <text x="18" y="106" font-size="7" fill="#1e293b">• ahorra peticiones HTTP (útil para íconos pequeños)</text>
  <text x="18" y="119" font-size="7" fill="#1e293b">• ideal para generar SVGs descargables desde JavaScript</text>
  <text x="18" y="131" font-size="7" fill="#1e293b">• permite enviar el SVG por email o incrustarlo en documentos</text>

  <!-- Desventajas -->
  <rect x="10" y="146" width="280" height="88" fill="#fee2e2" stroke="#ef4444" rx="3"/>
  <text x="18" y="162" font-size="9" font-weight="bold" fill="#991b1b">✗ Desventajas</text>
  <text x="18" y="177" font-size="7" fill="#1e293b">• base64 añade ~33% de peso al archivo original</text>
  <text x="18" y="190" font-size="7" fill="#1e293b">• no se cachea como recurso independiente por el navegador</text>
  <text x="18" y="203" font-size="7" fill="#1e293b">• hace el SVG ilegible al abrir en editor de texto</text>
  <text x="18" y="216" font-size="7" fill="#1e293b">• si la imagen se repite varias veces, aumenta peso proporcionalmente</text>
  <text x="18" y="229" font-size="7" fill="#1e293b">• cambiar la imagen obliga a regenerar toda la cadena</text>

  <text x="150" y="250" text-anchor="middle" font-size="7" fill="#94a3b8">
    regla: íconos pequeños y SVGs portables → sí; fotos grandes → no
  </text>
</svg>
```

---

## Cómo generar el data URI

```svg
<svg width="300" height="250"
     viewBox="0 0 300 250"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    herramientas para codificar
  </text>

  <!-- Bash/CLI -->
  <rect x="15" y="38" width="270" height="44" fill="#ede9fe" stroke="#8b5cf6" rx="3"/>
  <text x="25" y="53" font-size="8" font-weight="bold" fill="#5b21b6">terminal (Linux/macOS)</text>
  <text x="25" y="66" font-size="7" font-family="monospace" fill="#475569">base64 -w 0 icon.png</text>
  <text x="25" y="77" font-size="7" font-family="monospace" fill="#475569">printf "data:image/png;base64,"; base64 -w 0 icon.png</text>

  <!-- JavaScript navegador -->
  <rect x="15" y="90" width="270" height="44" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="105" font-size="8" font-weight="bold" fill="#1e40af">navegador (btoa)</text>
  <text x="25" y="118" font-size="7" font-family="monospace" fill="#475569">const svg = '&lt;svg ...&gt;&lt;/svg&gt;';</text>
  <text x="25" y="129" font-size="7" font-family="monospace" fill="#475569">const uri = 'data:image/svg+xml;base64,' + btoa(svg);</text>

  <!-- Node -->
  <rect x="15" y="142" width="270" height="44" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="157" font-size="8" font-weight="bold" fill="#166534">Node.js</text>
  <text x="25" y="170" font-size="7" font-family="monospace" fill="#475569">const buf = fs.readFileSync('icon.png');</text>
  <text x="25" y="181" font-size="7" font-family="monospace" fill="#475569">const uri = 'data:image/png;base64,' + buf.toString('base64');</text>

  <!-- URL-encode para SVG -->
  <rect x="15" y="194" width="270" height="44" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="209" font-size="8" font-weight="bold" fill="#92400e">SVG → URL-encoded (más compacto)</text>
  <text x="25" y="222" font-size="7" font-family="monospace" fill="#475569">const uri = 'data:image/svg+xml;utf8,' +</text>
  <text x="25" y="233" font-size="7" font-family="monospace" fill="#475569">  encodeURIComponent(svg);</text>
</svg>
```
