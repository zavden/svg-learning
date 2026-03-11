# 3.1 El viewport

El **viewport** es el espacio físico que ocupa el SVG en la pantalla. Se define con los atributos `width` y `height` del elemento `<svg>`. Piensa en él como el tamaño de la "caja" visible antes de saber nada del contenido interno.

Sin `viewBox`, las coordenadas internas coinciden directamente con los píxeles del viewport: `width="200"` significa 200 píxeles de ancho, y un elemento en `x="200"` está exactamente en el borde derecho.

- `width` y `height` aceptan: números (px implícito), `px`, `%`, `em`, `cm`, `mm`, `pt`, `pc`
- Si se omiten, el navegador usa `300×150 px` por defecto
- El contenido que excede el viewport se **recorta** (`overflow: hidden` por defecto)

---

## Ejemplo 1: Viewport fijo 200×200 — coordenadas igualan píxeles

Con `width="200" height="200"` y sin `viewBox`, cada unidad de coordenada es 1 píxel CSS exacto. El punto (0,0) está en la esquina superior izquierda; (200,200) en la inferior derecha.

```svg
<svg width="200" height="200"
     style="border:3px solid #3b82f6;background:#f8fafc;display:block;">
  <!-- viewport: 200×200px — el borde azul marca su límite físico -->
  <!-- Sin viewBox: 1 unidad de usuario = 1 píxel CSS -->

  <!-- Cuadrícula cada 50px para referencia -->
  <line x1="50"  y1="0"   x2="50"  y2="200"
        stroke="#e2e8f0" stroke-width="1"/>
  <line x1="100" y1="0"   x2="100" y2="200"
        stroke="#cbd5e1" stroke-width="1" stroke-dasharray="4,2"/>
  <line x1="150" y1="0"   x2="150" y2="200"
        stroke="#e2e8f0" stroke-width="1"/>
  <line x1="0"   y1="50"  x2="200" y2="50"
        stroke="#e2e8f0" stroke-width="1"/>
  <line x1="0"   y1="100" x2="200" y2="100"
        stroke="#cbd5e1" stroke-width="1" stroke-dasharray="4,2"/>
  <line x1="0"   y1="150" x2="200" y2="150"
        stroke="#e2e8f0" stroke-width="1"/>

  <!-- Esquinas del viewport: donde empieza y acaba el área física -->
  <circle cx="0"   cy="0"   r="5" fill="tomato"/>
  <circle cx="200" cy="200" r="5" fill="tomato"/>
  <text x="7"   y="17"  font-size="10" fill="tomato">(0,0)</text>
  <text x="126" y="197" font-size="10" fill="tomato">(200,200)</text>

  <!-- Etiqueta informativa en el centro -->
  <text x="100" y="107" text-anchor="middle"
        font-size="13" fill="#475569">200 × 200 px</text>
  <text x="100" y="123" text-anchor="middle"
        font-size="10" fill="#94a3b8">1 unidad = 1 px</text>
</svg>
```

---

## Ejemplo 2: Viewport apaisado 300×100 — cualquier proporción

El viewport puede tener cualquier proporción. Este SVG de `300×100` es 3 veces más ancho que alto. Los contenidos se dibujan en un espacio de 300×100 unidades.

```svg
<svg width="300" height="100"
     style="border:3px solid #10b981;background:#f0fdf4;display:block;">
  <!-- viewport: 300×100px → formato apaisado, ratio 3:1 -->

  <!-- Cuadrícula cada 50px -->
  <line x1="50"  y1="0" x2="50"  y2="100"
        stroke="#d1fae5" stroke-width="1"/>
  <line x1="100" y1="0" x2="100" y2="100"
        stroke="#d1fae5" stroke-width="1"/>
  <line x1="150" y1="0" x2="150" y2="100"
        stroke="#a7f3d0" stroke-width="1" stroke-dasharray="4,2"/>
  <line x1="200" y1="0" x2="200" y2="100"
        stroke="#d1fae5" stroke-width="1"/>
  <line x1="250" y1="0" x2="250" y2="100"
        stroke="#d1fae5" stroke-width="1"/>
  <line x1="0"   y1="50" x2="300" y2="50"
        stroke="#a7f3d0" stroke-width="1" stroke-dasharray="4,2"/>

  <!-- Esquinas del viewport -->
  <circle cx="0"   cy="0"   r="4" fill="#10b981"/>
  <circle cx="300" cy="0"   r="4" fill="#10b981"/>
  <circle cx="0"   cy="100" r="4" fill="#10b981"/>
  <circle cx="300" cy="100" r="4" fill="#10b981"/>
  <text x="5"   y="14" font-size="9" fill="#065f46">(0,0)</text>
  <text x="228" y="97" font-size="9" fill="#065f46">(300,100)</text>

  <!-- Etiqueta -->
  <text x="150" y="54" text-anchor="middle"
        font-size="11" fill="#065f46">300 × 100 px (ratio 3:1)</text>
</svg>
```

---

## Ejemplo 3: width="100%" — viewport responsivo en anchura

Con `width="100%"` el SVG ocupa todo el ancho de su contenedor CSS. La altura puede ser fija o también relativa. Arrastra el divisor del previewer para ver cómo se adapta.

```svg
<svg width="100%" height="110"
     style="border:3px solid #f59e0b;background:#fffbeb;display:block;">
  <!-- width="100%" → el SVG ocupa el 100% del ancho del contenedor -->
  <!-- height="110" → la altura sigue siendo 110px fijos -->
  <!-- NOTA: sin viewBox, las coords internas NO escalan con el ancho -->
  <!-- Un rect con width="100%" sí escala; uno con width="200" no -->

  <!-- Esta barra usa width="100%" y por tanto sí escala -->
  <rect x="0" y="28" width="100%" height="28"
        fill="#fde68a" rx="3"/>
  <text x="12" y="47" font-size="11" fill="#92400e">
    Este rect (width="100%") escala con el SVG
  </text>

  <!-- Línea en el 50% del ancho -->
  <line x1="50%" y1="0" x2="50%" y2="110"
        stroke="#f59e0b" stroke-width="2" stroke-dasharray="5,3"/>
  <text x="12" y="82" font-size="10" fill="#b45309">
    width="100%" height="110"
  </text>
  <text x="12" y="98" font-size="10" fill="#92400e">
    Mueve el divisor del previewer para ver el efecto →
  </text>
</svg>
```

---

## Ejemplo 4: El viewport recorta — overflow:hidden por defecto

El SVG aplica `overflow: hidden` automáticamente: todo lo que esté fuera del rectángulo del viewport desaparece. Los elementos siguen existiendo en el árbol SVG, simplemente no se renderizan.

```svg
<svg width="200" height="140"
     style="border:3px solid #3b82f6;background:#f0f4f8;display:block;">
  <!-- viewport: 200×140px — el borde azul marca el límite de recorte -->
  <!-- overflow: hidden por defecto → lo que excede este rect, no se ve -->

  <!-- Círculo completamente DENTRO: visible al 100% -->
  <circle cx="70" cy="70" r="48" fill="#3b82f6" opacity="0.85"/>
  <text x="70" y="74" text-anchor="middle"
        font-size="11" fill="white">visible</text>

  <!-- Círculo PARCIALMENTE fuera por la derecha: se recorta -->
  <circle cx="188" cy="40" r="42" fill="#f59e0b" opacity="0.85"/>
  <text x="180" y="42" text-anchor="middle"
        font-size="9" fill="white">recorta</text>

  <!-- Círculo PARCIALMENTE fuera por abajo: también recortado -->
  <circle cx="148" cy="138" r="36" fill="#10b981" opacity="0.85"/>
  <text x="148" y="140" text-anchor="middle"
        font-size="9" fill="white">recorta</text>

  <!-- Marco del viewport para referencia -->
  <rect width="200" height="140" fill="none"
        stroke="#3b82f6" stroke-width="1.5" stroke-dasharray="5,3"/>
  <text x="100" y="133" text-anchor="middle"
        font-size="9" fill="#475569">overflow:hidden (por defecto)</text>
</svg>
```

---

## Ejemplo 5: overflow="visible" — el contenido desborda el viewport

Añadiendo `overflow="visible"` al elemento `<svg>`, los elementos fuera del viewport siguen renderizándose. El área física del SVG (que afecta al layout de la página) no cambia.

```svg
<svg width="200" height="140" overflow="visible"
     style="border:3px solid #8b5cf6;background:#faf5ff;
            display:block;margin:30px auto;">
  <!-- overflow="visible" → el contenido fuera del viewport SÍ se ve -->
  <!-- El borde morado sigue siendo el viewport físico (200×140px) -->
  <!-- margin:30px deja espacio visual para el contenido que desborda -->

  <!-- Círculo central: completamente dentro del viewport -->
  <circle cx="100" cy="70" r="48" fill="#8b5cf6" opacity="0.9"/>
  <text x="100" y="74" text-anchor="middle"
        font-size="11" fill="white">dentro</text>

  <!-- Círculo desbordando hacia la derecha: visible con overflow -->
  <circle cx="218" cy="38" r="38" fill="#f59e0b" opacity="0.85"/>
  <text x="218" y="41" text-anchor="middle"
        font-size="9" fill="white">→ fuera</text>

  <!-- Círculo desbordando hacia abajo -->
  <circle cx="55" cy="175" r="32" fill="#10b981" opacity="0.85"/>
  <text x="55" y="178" text-anchor="middle"
        font-size="9" fill="white">↓ fuera</text>

  <!-- Marco del viewport para dejar claro su límite -->
  <rect width="200" height="140" fill="none"
        stroke="#8b5cf6" stroke-width="2" stroke-dasharray="6,3"/>
</svg>
```

---

## Ejemplo 6: Mismo contenido, distintos viewports — el tamaño físico cambia

Sin `viewBox`, el tamaño del viewport determina el tamaño físico del contenido. El mismo elemento dibujado con las mismas coordenadas se ve distinto según el tamaño del viewport.

```svg
<svg width="280" height="160"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block;">
  <!-- Sin viewBox: cambiar el viewport = cambiar el tamaño visual -->
  <!-- Tres "casos" simulados: mismos objetos, distintos tamaños de caja -->

  <text x="140" y="14" text-anchor="middle"
        font-size="10" fill="#475569">
    Sin viewBox: viewport fija el tamaño del contenido
  </text>

  <!-- Caso 1: viewport pequeño (60×60) -->
  <rect x="10"  y="22" width="60"  height="60"
        fill="#eff6ff" stroke="#3b82f6" stroke-width="2" rx="2"/>
  <circle cx="40"  cy="52" r="22" fill="#3b82f6" opacity="0.8"/>
  <text x="40"  y="97" text-anchor="middle"
        font-size="9" fill="#64748b">60×60</text>

  <!-- Caso 2: viewport mediano (90×90) -->
  <rect x="85"  y="22" width="90"  height="90"
        fill="#f0fdf4" stroke="#10b981" stroke-width="2" rx="2"/>
  <circle cx="130" cy="67" r="33" fill="#10b981" opacity="0.8"/>
  <text x="130" y="127" text-anchor="middle"
        font-size="9" fill="#64748b">90×90</text>

  <!-- Caso 3: viewport grande (110×110) -->
  <rect x="190" y="22" width="80"  height="80"
        fill="#fffbeb" stroke="#f59e0b" stroke-width="2" rx="2"/>
  <circle cx="230" cy="62" r="30" fill="#f59e0b" opacity="0.8"/>
  <text x="230" y="117" text-anchor="middle"
        font-size="9" fill="#64748b">80×80</text>

  <text x="140" y="150" text-anchor="middle"
        font-size="10" fill="#94a3b8">
    Con viewBox el contenido siempre llena el viewport
  </text>
</svg>
```
