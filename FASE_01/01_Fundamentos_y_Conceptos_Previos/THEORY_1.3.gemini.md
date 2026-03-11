# 1.3 Cuándo usar SVG y cuándo no

SVG es poderoso, pero no es la 'bala de plata' para toda la web. Elegir el 
formato visual correcto dictaminará si el "Main Thread" de tu aplicación corre 
fluido a 60FPS o colapsa miserablemente.

A continuación exploraremos los entornos donde el SVG es el rey y los límites 
matemáticos y de RAM donde debemos cambiar a mapas de bits (Raster) o `<canvas>`.

---

## SVG Diga "Sí": Iconos y Componentes UI Reactivos

Los iconos y la interfaz gráfica son el uso más popular de SVG. Al ser expuestos 
al DOM, son perfectos para librerías como React, Vue o Angular. Permiten que 
el **estado de la aplicación (State)** modifique la estructura visual (cambiando 
parámetros internos de un gráfico directamente sin recargar imágenes nuevas).

### Ejemplo 1: Puntero Intuitivo (Estado Limpio)
Un vector limpio transfiere poco byte a la red y escala sin romperse en Retina.
```svg
<svg width="150" height="80" xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0; background:#f8fafc; display:block;">
  <g transform="translate(60, 20)">
    <!-- Ícono home -->
    <polygon points="0,-25 25,0 20,0 20,22 -20,22 -20,0 -25,0" fill="#3b82f6"/>
    <rect x="-10" y="5" width="20" height="17" fill="white" opacity="0.9"/>
  </g>
</svg>
```

### Ejemplo 2: Variables y Feedback Sensorial Visual
Puedes mapear estados nativos de interacción (como `hover` o `active`) mediante 
CSS o JavaScript hacia las paredes matemáticas (strokes/fills) del icono.
```svg
<svg width="150" height="80" xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0; background:#f8fafc; display:block;">
  <style>
    .star { fill: #cbd5e1; transition: fill 0.3s; cursor: pointer; }
    .star:hover { fill: #f59e0b; } /* Estado activo = Dorado */
  </style>
  <polygon class="star" 
           points="75,15 85,45 115,45 90,65 100,95 75,75 50,95 60,65 35,45 65,45"
           transform="scale(0.8) translate(15,-5)"/>
</svg>
```

### Ejemplo 3: Iconos Compuestos y Semántica 
SVG permite anidaciones múltiples `<svg> -> <g>`. Los grupos combinan 
primitivas geométricas en un único ente semántico controlable.
```svg
<svg width="150" height="80" xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0; background:#f8fafc; display:block;">
  <g transform="translate(75, 40)">
    <!-- Base Azul -->
    <circle r="22" fill="#6366f1"/>
    <!-- Detalles Blanco construyen el Usuario de UI -->
    <circle cx="0" cy="-5" r="8" fill="white"/>
    <ellipse cx="0" cy="18" rx="13" ry="9" fill="white"/>
  </g>
</svg>
```

---

## SVG Diga "Sí": Gráficos de Datos "Dom-Bound" (Hasta 10K Nodos)

SVG es el estándar para visualización y gráficas interactivas D3.js, Chart.js, etc.
Sin embargo, hay un límite: **El Cuello de Botella del DOM (DOM Bottleneck)**.
Dado que SVG funciona como un lenguaje "Retained Mode", cada cuadrado de tu gráfico 
es un Objeto rastreado por la memoria RAM del navegador. Si renderizas un gráfico 
de barras con 50 elementos, SVG brilla. Si intentas renderizar un Scatter Plot
con 100,000 partículas físicas, colapsarás el Main Thread y deberás usar WebGL`<canvas>`.

### Ejemplo 4: Gráfico Barras Interactivo (Retained Mode Perfecto)
Este diagrama mantiene sus ejes y barras como objetos individuales. Una 
librería JavaScript puede animar su altura o adjuntar "Tooltips" sin redibujar todo.
```svg
<svg width="200" height="120" xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0; background:#f8fafc; display:block;">
  <style>
    .bar { transition: transform 0.3s ease, opacity 0.3s; }
    .bar:hover { opacity: 0.7; transform: translateY(-5px); cursor:pointer;}
  </style>
  <!-- Eje base -->
  <line x1="20" y1="100" x2="180" y2="100" stroke="#94a3b8" />
  
  <rect x="40" y="50" width="30" height="50" fill="#3b82f6" class="bar"/>
  <rect x="85" y="30" width="30" height="70" fill="#10b981" class="bar"/>
  <rect x="130" y="60" width="30" height="40" fill="#f59e0b" class="bar"/>
</svg>
```

### Ejemplo 5: Diagramas de Líneas y Continuidad (Paths Data)
Para pintar tendencias temporales continuas, un solo elemento `path` puede recorrer
el "viewport" describiendo visualmente cientos de datapoints y calculando 
Béziers en GPU.
```svg
<svg width="200" height="120" xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0; background:#f8fafc; display:block;">
  <line x1="20" y1="20" x2="20" y2="100" stroke="#94a3b8" />
  <line x1="20" y1="100" x2="180" y2="100" stroke="#94a3b8" />
  
  <path d="M 20 90 Q 60 20, 100 60 T 180 30" 
        fill="none" stroke="#ef4444" stroke-width="3" />
</svg>
```

---

## SVG Diga "Sí": Fondos y "Wave Dividers" CSS

Las ondas divisorias que se han vuelto un estándar del diseño web moderno
dependen 100% de SVG, a menudo incrustado como código Base64 mediante estilos
`background-image: url("data:image/svg+xml,...")`. 
¿Por qué? Porque si redimensionas una pestaña web (Viewport resizing), la matemática
pinta la onda a lo ancho sin distorsionarla, cosa imposible con un bitmap.

### Ejemplo 6: Onda Divisoria de Interfaz Plana
Un sólo elemento path describiendo curvas polinómicas Cúbicas.
```svg
<svg width="300" height="100" xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0; background:#f8fafc; display:block;">
  <path d="M0,50 C75,0 150,100 300,50 L300,100 L0,100 Z" 
        fill="#3b82f6" opacity="0.6"/>
</svg>
```

### Ejemplo 7: Texturizado de Fondos Geométricos (<pattern>)
SVG cuenta nativamente con herramientas increíbles para generar azulejos repetitivos.
Ahorrándote cargar "texturas de patrón de punto" PNG pesados.
```svg
<svg width="300" height="100" xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0; background:#f8fafc; display:block;">
  <defs>
    <!-- Patrón reutilizable cada 20x20 math bounds -->
    <pattern id="dotPattern" width="20" height="20" patternUnits="userSpaceOnUse">
      <circle cx="10" cy="10" r="3" fill="#cbd5e1" />
    </pattern>
  </defs>
  <!-- Usa el patrón infinito como relleno de la caja -->
  <rect width="300" height="100" fill="url(#dotPattern)" />
</svg>
```

---

## SVG Diga "NO": Fotografías y Arte Realista

Una fotografía tiene millones de píxeles independientes con transiciones continuas. 
Una imagen PNG/JPEG es excelente administrando y comprimiendo "mapas físicos finitos
de píxeles finitos".

Tratar de forzar contenido orgánico dentro de SVG implicaría modelar cada píxel como
un elemento nodal `<rect>`. Esto engordaría el archivo hasta hundir la red
(50 KB crashearían hasta volverse de 15 MB) y colapsar la RAM del pobre navegador 
calculando el "Bounding Box" innecesario de cada píxel individual.

### Ejemplo 8: Correcto (Para logos/Vector usar SVG)
Un logo geométrico transfiere instantáneo y vive feliz en RAM.
```svg
<svg width="200" height="100" xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0; background:#f8fafc; display:block;">
  <text x="100" y="30" text-anchor="middle" font-size="12" fill="#1e293b">
    LOGO CORPORATIVO
  </text>
  <polyline points="70,80 100,45 130,80" fill="none" 
            stroke="#10b981" stroke-width="6" stroke-linecap="round"/>
</svg>
```

### Ejemplo 9: Incorrecto (Intentando falsear una fotografía en SVG)
Imagina si este ejemplo de 9 cuadros intentara escalar a una foto 1920x1080px...
¡Crearia +2 millones de nodos DOM `<rect>` en tu SVG destruyendo el motor Blink/V8!
```svg
<svg width="200" height="100" xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0; background:#f8fafc; display:block;">
  <text x="100" y="30" text-anchor="middle" font-size="10" fill="#ef4444">
    Un SVG haciendo el trabajo de un JPG/PNG
  </text>
  <!-- Simulando pixeles falsos: Un desperdicio atroz de CPU del DOM -->
  <g transform="translate(65, 40)">
    <rect x="0" y="0" width="10" height="10" fill="#92400e"/>
    <rect x="10" y="0" width="10" height="10" fill="#78716c"/>
    <rect x="20" y="0" width="10" height="10" fill="#a3e635"/>
    <rect x="0" y="10" width="10" height="10" fill="#60a5fa"/>
    <rect x="10" y="10" width="10" height="10" fill="#34d399"/>
    <rect x="20" y="10" width="10" height="10" fill="#fb923c"/>
    <rect x="0" y="20" width="10" height="10" fill="#e879f9"/>
    <rect x="10" y="20" width="10" height="10" fill="#f87171"/>
    <rect x="20" y="20" width="10" height="10" fill="#cbd5e1"/>
  </g>
</svg>
```
