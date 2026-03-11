# 3.2 El atributo `viewBox`

El `viewBox` es el atributo más importante de SVG. Desacopla el **sistema de coordenadas interno** del SVG del espacio físico que ocupa en pantalla. Sin él, las coordenadas son píxeles directos. Con él, puedes definir tu propio "mapa" y el navegador se encarga de la conversión automáticamente.

**Sintaxis:** `viewBox="min-x min-y ancho alto"`

Los cuatro valores definen el rectángulo del espacio de usuario que será visible:
- `min-x, min-y`: coordenada de la esquina superior izquierda del área visible
- `ancho, alto`: dimensiones del área visible en unidades de usuario

---

## Ejemplo 1: Sin viewBox — las coordenadas son píxeles directamente

Cuando no hay `viewBox`, cada unidad de coordenada equivale a 1 píxel CSS. El SVG de 200×200 tiene coordenadas que van de 0 a 200 directamente en píxeles. El círculo con `cx="100" cy="100"` está exactamente en el centro visual.

```svg
<svg width="200" height="200"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block;">
  <!-- Sin viewBox: 1 unidad = 1 píxel CSS -->
  <!-- Este SVG tiene 200×200 píxeles de viewport -->

  <!-- Cuadrícula de referencia cada 50px -->
  <line x1="50"  y1="0" x2="50"  y2="200"
        stroke="#e2e8f0" stroke-width="1"/>
  <line x1="100" y1="0" x2="100" y2="200"
        stroke="#cbd5e1" stroke-width="1"/>
  <line x1="150" y1="0" x2="150" y2="200"
        stroke="#e2e8f0" stroke-width="1"/>
  <line x1="0" y1="50"  x2="200" y2="50"
        stroke="#e2e8f0" stroke-width="1"/>
  <line x1="0" y1="100" x2="200" y2="100"
        stroke="#cbd5e1" stroke-width="1"/>
  <line x1="0" y1="150" x2="200" y2="150"
        stroke="#e2e8f0" stroke-width="1"/>

  <!-- Círculo centrado: cx=100, cy=100 = el centro exacto -->
  <!-- del SVG de 200×200 -->
  <circle cx="100" cy="100" r="60" fill="steelblue" opacity="0.85"/>

  <!-- Origen (0,0) en la esquina superior izquierda -->
  <circle cx="0" cy="0" r="4" fill="tomato"/>
  <text x="6" y="14" font-size="11" fill="tomato">(0,0)</text>

  <!-- Esquina opuesta: (200,200) -->
  <circle cx="200" cy="200" r="4" fill="tomato"/>

  <!-- Centro: (100,100) -->
  <circle cx="100" cy="100" r="4" fill="white"/>
  <text x="108" y="98" font-size="10" fill="white">cx=100, cy=100</text>
</svg>
```

---

## Ejemplo 2: viewBox más pequeño que el viewport → Zoom in

El viewport sigue siendo 200×200 píxeles, pero el `viewBox` define un espacio de 50×50 unidades. El navegador estira esas 50 unidades para llenar los 200 píxeles. **Factor de escala: 200/50 = 4x**. Cada unidad de usuario ocupa 4 píxeles.

```svg
<svg width="200" height="200" viewBox="0 0 50 50"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block;">
  <!-- viewport: 200×200px | viewBox: 50×50 unidades -->
  <!-- Escala: 4x → cada unidad de usuario = 4 píxeles en pantalla -->

  <!-- El centro del viewBox está en (25, 25), no en (100, 100) -->
  <circle cx="25" cy="25" r="15" fill="steelblue" opacity="0.85"/>

  <!-- r=15 unidades × 4 = 60px en pantalla -->
  <text x="25" y="28" text-anchor="middle" font-size="4" fill="white">
    r=15 → 60px
  </text>

  <!-- Las cuadrículas cada 10 unidades ahora ocupan 40px -->
  <line x1="10" y1="0" x2="10" y2="50" stroke="#e2e8f0" stroke-width="0.5"/>
  <line x1="20" y1="0" x2="20" y2="50" stroke="#e2e8f0" stroke-width="0.5"/>
  <line x1="30" y1="0" x2="30" y2="50" stroke="#e2e8f0" stroke-width="0.5"/>
  <line x1="40" y1="0" x2="40" y2="50" stroke="#e2e8f0" stroke-width="0.5"/>
  <line x1="0" y1="10" x2="50" y2="10" stroke="#e2e8f0" stroke-width="0.5"/>
  <line x1="0" y1="20" x2="50" y2="20" stroke="#e2e8f0" stroke-width="0.5"/>
  <line x1="0" y1="30" x2="50" y2="30" stroke="#e2e8f0" stroke-width="0.5"/>
  <line x1="0" y1="40" x2="50" y2="40" stroke="#e2e8f0" stroke-width="0.5"/>

  <!-- Origen: (0,0) en el viewBox → (0px, 0px) en pantalla -->
  <circle cx="0" cy="0" r="2" fill="tomato"/>

  <!-- Nota: font-size también escala → font-size="4" → 16px en pantalla -->
  <text x="2" y="7" font-size="3" fill="#64748b">viewBox: 0 0 50 50</text>
</svg>
```

---

## Ejemplo 3: viewBox más grande que el viewport → Zoom out

El viewport es 200×200 píxeles, pero el `viewBox` es 400×400 unidades. El navegador comprime 400 unidades en 200 píxeles. **Factor de escala: 0.5x**. Cada unidad ocupa medio píxel.

```svg
<svg width="200" height="200" viewBox="0 0 400 400"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block;">
  <!-- viewport: 200×200px | viewBox: 400×400 unidades -->
  <!-- Escala: 0.5x → cada unidad de usuario = 0.5 píxeles en pantalla -->

  <!-- Cuadrícula cada 100 unidades (= 50px en pantalla) -->
  <line x1="100" y1="0" x2="100" y2="400"
        stroke="#e2e8f0" stroke-width="1"/>
  <line x1="200" y1="0" x2="200" y2="400"
        stroke="#cbd5e1" stroke-width="2"/>
  <line x1="300" y1="0" x2="300" y2="400"
        stroke="#e2e8f0" stroke-width="1"/>
  <line x1="0" y1="100" x2="400" y2="100"
        stroke="#e2e8f0" stroke-width="1"/>
  <line x1="0" y1="200" x2="400" y2="200"
        stroke="#cbd5e1" stroke-width="2"/>
  <line x1="0" y1="300" x2="400" y2="300"
        stroke="#e2e8f0" stroke-width="1"/>

  <!-- El centro está en (200,200) del viewBox = (100px, 100px) en pantalla -->
  <circle cx="200" cy="200" r="120" fill="steelblue" opacity="0.85"/>

  <!-- r=120 unidades × 0.5 = 60px en pantalla -->
  <!-- (igual que el ejemplo anterior) -->
  <text x="200" y="205" text-anchor="middle" font-size="18" fill="white">
    r=120 → 60px
  </text>

  <!-- Etiquetas de las líneas de cuadrícula -->
  <text x="105" y="15" font-size="12" fill="#94a3b8">100</text>
  <text x="205" y="15" font-size="12" fill="#94a3b8">200</text>
  <text x="305" y="15" font-size="12" fill="#94a3b8">300</text>
</svg>
```

---

## Ejemplo 4: Desplazamiento con min-x y min-y — efecto "cámara"

Los valores `min-x` y `min-y` mueven el encuadre. Es como desplazar la cámara: el contenido del SVG no cambia, solo cambia qué parte ves. Con `viewBox="100 100 200 200"`, el área visible **empieza** en la coordenada (100,100) del espacio de usuario.

```svg
<svg width="300" height="300" viewBox="100 100 200 200"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block;">
  <!-- El espacio de coordenadas total: los círculos están en -->
  <!-- (50,50), (200,200), (320,320) -->
  <!-- Solo vemos la región entre (100,100) y (300,300) -->

  <!-- Este círculo está en (50,50): FUERA del área visible -->
  <!-- (no se renderiza) -->
  <circle cx="50" cy="50" r="35" fill="#94a3b8" opacity="0.4"/>
  <text x="50" y="54" text-anchor="middle" font-size="14" fill="#94a3b8">
    FUERA
  </text>

  <!-- Líneas que marcan el borde del viewBox para referencia -->
  <rect x="100" y="100" width="200" height="200"
        fill="none" stroke="#fbbf24"
        stroke-width="2" stroke-dasharray="6,3"/>

  <!-- Este círculo está en (170,170): visible (dentro del viewBox) -->
  <circle cx="170" cy="170" r="45" fill="steelblue" opacity="0.85"/>
  <text x="170" y="175" text-anchor="middle" font-size="12" fill="white">
    (170,170) visible
  </text>

  <!-- Este círculo está en (270,270): también visible -->
  <circle cx="270" cy="270" r="30" fill="tomato" opacity="0.85"/>
  <text x="270" y="274" text-anchor="middle" font-size="10" fill="white">
    (270,270)
  </text>

  <!-- Este círculo está parcialmente fuera (en x=320): se corta -->
  <circle cx="320" cy="160" r="40" fill="#a855f7" opacity="0.7"/>
  <text x="300" y="162" font-size="10" fill="#a855f7">parcial</text>
</svg>
```

---

## Ejemplo 5: SVG responsivo — viewBox sin dimensiones fijas

La combinación más poderosa: `viewBox` sin `width`/`height` en el elemento `<svg>`. El SVG ocupa el 100% del contenedor CSS y mantiene sus proporciones automáticamente. Las coordenadas internas son siempre 0-200 × 0-100 sin importar el tamaño de la pantalla.

```svg
<svg viewBox="0 0 200 100"
     style="width:100%;height:auto;border:2px solid #e2e8f0;
            background:#f8fafc;display:block;">
  <!-- Sin width/height fijos en el SVG: el CSS controla el tamaño exterior -->
  <!-- viewBox establece las proporciones: 200:100 = ratio 2:1 -->
  <!-- Los elementos se posicionan en un espacio de 0-200 horizontal, -->
  <!-- 0-100 vertical -->
  <!-- Siempre, independientemente del tamaño real en pantalla -->

  <!-- Fondo del gráfico -->
  <rect width="200" height="100" fill="#f0f4f8"/>
  <line x1="10" y1="90" x2="195" y2="90"
        stroke="#94a3b8" stroke-width="1.5"/>

  <!-- Barras que siempre llenan el espacio disponible -->
  <rect x="15"  y="55" width="22" height="35" fill="#3b82f6" rx="2"/>
  <rect x="45"  y="35" width="22" height="55" fill="#3b82f6" rx="2"/>
  <rect x="75"  y="65" width="22" height="25" fill="#3b82f6" rx="2"/>
  <rect x="105" y="25" width="22" height="65" fill="#ef4444" rx="2"/>
  <rect x="135" y="45" width="22" height="45" fill="#3b82f6" rx="2"/>
  <rect x="165" y="20" width="22" height="70" fill="#ef4444" rx="2"/>

  <!-- Etiqueta -->
  <text x="100" y="11" text-anchor="middle" font-size="7" fill="#475569">
    Este SVG escala con el contenedor — las coordenadas siempre van de 0 a 200
  </text>
</svg>
```

---

## Ejemplo 6: viewBox para recortar y enfocar una región específica

Cambiando el `viewBox` podemos actuar como una "lupa" o "zoom" sobre una parte del gráfico. El contenido completo existe en el espacio de coordenadas, pero solo mostramos la región definida por el `viewBox`.

```svg
<svg width="300" height="200" viewBox="60 15 120 80"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block;">
  <!-- El gráfico completo tiene coordenadas de 0-300 × 0-200 -->
  <!-- viewBox="60 15 120 80": solo mostramos la región central -->
  <!-- Visible: x de 60 a 180, y de 15 a 95 -->

  <!-- Fondo del gráfico completo -->
  <rect width="300" height="200" fill="#f0f4f8"/>

  <!-- Múltiples formas dispersas por todo el espacio de coordenadas -->
  <!-- Las de fuera del viewBox simplemente no se ven -->
  <circle cx="25"  cy="100" r="18" fill="#74b9ff" opacity="0.9"/>
  <!-- VISIBLE -->
  <circle cx="95"  cy="40"  r="22" fill="#e74c3c" opacity="0.9"/>
  <!-- VISIBLE -->
  <circle cx="145" cy="70"  r="18" fill="#00b894" opacity="0.9"/>
  <circle cx="200" cy="130" r="25" fill="#fdcb6e" opacity="0.9"/>
  <circle cx="260" cy="55"  r="20" fill="#6c5ce7" opacity="0.9"/>

  <!-- Marco que indica la región del viewBox (para referencia visual) -->
  <rect x="60" y="15" width="120" height="80"
        fill="none" stroke="#fbbf24"
        stroke-width="2" stroke-dasharray="5,3"/>

  <!-- Etiqueta fuera del viewBox (no se verá) -->
  <text x="150" y="180" text-anchor="middle" font-size="16" fill="#94a3b8">
    Texto fuera del viewBox
  </text>
</svg>
```

---

## Ejemplo 7: La fórmula de conversión — de coordenadas a píxeles

El navegador aplica esta fórmula para convertir coordenadas de usuario a píxeles: `pixel = (coord / tamaño_viewBox) × tamaño_viewport`. Con un viewport de 300×200 y un viewBox de 100×66, la escala es ×3 en ambos ejes.

```svg
<svg width="300" height="200" viewBox="0 0 100 66"
     style="border:2px solid #e2e8f0;display:block;font-family:monospace;">
  <!-- viewport: 300×200px | viewBox: 100×66 unidades -->
  <!-- Escala X: 300/100 = 3   |   Escala Y: 200/66 ≈ 3.03 -->

  <!-- Cuadrícula de referencia cada 10 unidades de usuario -->
  <!-- (= 30px en pantalla) -->
  <defs>
    <pattern id="cuadricula" width="10" height="10"
             patternUnits="userSpaceOnUse">
      <path d="M 10 0 L 0 0 0 10"
            fill="none" stroke="#e2e8f0" stroke-width="0.4"/>
    </pattern>
  </defs>
  <rect width="100" height="66" fill="#f8fafc"/>
  <rect width="100" height="66" fill="url(#cuadricula)"/>

  <!-- Ejes -->
  <line x1="0" y1="0" x2="100" y2="0" stroke="#cbd5e1" stroke-width="1"/>
  <line x1="0" y1="0" x2="0" y2="66" stroke="#cbd5e1" stroke-width="1"/>

  <!-- Punto (0,0) → pixel (0, 0) = esquina superior izquierda -->
  <circle cx="0" cy="0" r="2.5" fill="tomato"/>
  <text x="2" y="7" font-size="4" fill="tomato">(0,0) → 0px,0px</text>

  <!-- Punto (50,33) → pixel (150, 100) = centro exacto -->
  <circle cx="50" cy="33" r="2.5" fill="steelblue"/>
  <line x1="0" y1="33" x2="50" y2="33"
        stroke="steelblue" stroke-width="0.5" stroke-dasharray="2,1"/>
  <line x1="50" y1="0" x2="50" y2="33"
        stroke="steelblue" stroke-width="0.5" stroke-dasharray="2,1"/>
  <text x="30" y="29" font-size="3.8" fill="steelblue">
    (50,33) → 150px,100px
  </text>

  <!-- Punto (100,66) → pixel (300, 200) = esquina opuesta -->
  <circle cx="100" cy="66" r="2.5" fill="#10b981"/>
  <text x="62" y="63" font-size="4" fill="#10b981">
    (100,66) → 300px,200px
  </text>

  <!-- Punto arbitrario: (25,50) → pixel (75, 151) -->
  <circle cx="25" cy="50" r="2" fill="#a855f7"/>
  <text x="27" y="50.5" font-size="3.5" fill="#a855f7">
    (25,50) → 75px,151px
  </text>

  <!-- Fórmula en el centro del gráfico -->
  <rect x="15" y="15" width="72" height="12" rx="2"
        fill="rgba(255,255,255,0.85)"/>
  <text x="51" y="23" text-anchor="middle" font-size="4" fill="#334155">
    pixel = (coord / viewBox) × viewport
  </text>
</svg>
```

---

## Ejemplo 8: Zoom interactivo — modificando el viewBox con JavaScript

El `viewBox` puede cambiarse dinámicamente con JavaScript. Este es el principio detrás de los controles de zoom y pan en librerías de visualización. Haz clic en el SVG para alternar entre la vista completa y un zoom 2× al cuadrante central.

```svg
<svg id="svg-zoom-demo"
     width="280" height="280"
     viewBox="0 0 100 100"
     style="border:2px solid #e2e8f0;background:#f8fafc;
            display:block;cursor:zoom-in;"
     onclick="
       var vb = this.getAttribute('viewBox');
       if (vb === '0 0 100 100') {
         this.setAttribute('viewBox', '25 25 50 50');
         this.style.cursor = 'zoom-out';
       } else {
         this.setAttribute('viewBox', '0 0 100 100');
         this.style.cursor = 'zoom-in';
       }
     ">
  <!-- viewBox="0 0 100 100" → vista completa (todos los círculos visibles) -->
  <!-- viewBox="25 25 50 50" → zoom 2x al cuadrante central -->

  <!-- Círculos en las cuatro esquinas -->
  <circle cx="15" cy="15" r="10" fill="#74b9ff"/>
  <circle cx="85" cy="15" r="10" fill="#fd79a8"/>
  <circle cx="15" cy="85" r="10" fill="#55efc4"/>
  <circle cx="85" cy="85" r="10" fill="#ffeaa7"/>

  <!-- Círculo central (siempre visible con el zoom al 25-75) -->
  <circle cx="50" cy="50" r="18" fill="steelblue" opacity="0.9"/>
  <text x="50" y="54" text-anchor="middle" font-size="7" fill="white">
    centro
  </text>

  <!-- Marco que indica la zona de zoom 2x -->
  <rect x="25" y="25" width="50" height="50"
        fill="rgba(251,191,36,0.1)"
        stroke="#fbbf24" stroke-width="1.5" stroke-dasharray="4,2"/>

  <!-- Instrucción -->
  <text x="50" y="97" text-anchor="middle" font-size="5" fill="#64748b">
    Clic para zoom 2× al centro
  </text>
</svg>
```
