# 1.2 Ventajas de SVG

SVG no es simplemente "otro formato de imagen". Tiene propiedades 
arquitectónicas y matemáticas que lo separan radicalmente de los gráficos 
rasterizados (PNG, JPEG, WebP). 

Para dominar SVG, hay que entender estas ventajas desde el punto de vista del 
motor del navegador y del ciclo de vida del desarrollo de software.

---

## Escalabilidad Perfecta y el "Retained Mode"

A diferencia del elemento `<canvas>` que funciona bajo un paradigma de 
**"Immediate Mode"** (donde dibujas y el navegador olvida, dejando solo píxeles),
SVG funciona bajo **"Retained Mode"**. 

El motor de renderizado **retiene el árbol de objetos en memoria**. Al hacer zoom, 
la GPU recalcula la *fórmula matemática*, manteniendo nitidez infinita.

### Ejemplo 1: Escalabilidad Vectorial (Círculo Pequeño)
```svg
<svg width="100" height="100" xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0; background:#f8fafc; display:block;">
  <!-- Círculo trazado a 16px -->
  <circle cx="50" cy="50" r="16" fill="#3b82f6"/>
</svg>
```

### Ejemplo 2: Escalabilidad Vectorial (Círculo Grande)
```svg
<svg width="200" height="200" xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0; background:#f8fafc; display:block;">
  <!-- La geometría se recalcula perfecta a r=80 -->
  <circle cx="100" cy="100" r="80" fill="#3b82f6"/>
</svg>
```

### Ejemplo 3: Escalabilidad Anisotrópica (Zoom sin distorsión)
Si escalamos el contenedor HTML, el SVG recalcula limpiamente sus bordes sin
pixelarse.
```svg
<svg width="300" height="100" xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0; background:#f8fafc; display:block;"
     viewBox="0 0 150 50">
  <!-- Escala el viewbox 150x50 a un contenedor 300x100 (2x) de forma pura -->
  <rect x="10" y="10" width="130" height="30" rx="5" fill="#10b981" />
</svg>
```

---

## Integración con el DOM, Eventos y Accesibilidad (AOM)

Un gráfico PNG no tiene un "adentro". SVG es un **documento XML vivo**. 
Sus nodos (`<rect>`, `<text>`) se inyectan directamente en el **DOM**.

1. **Hit-Testing:** Un evento `click` sobre una estrella SVG solo dispara si 
   el cursor toca literalmente la pintura matemática (no la caja contenedora).
2. **Accesibilidad (AOM):** La etiqueta `<text>` se indexa por el lector de 
   pantalla (Screen Reader) y puede seleccionarse; no es imagen de texto.
3. **Punteros Transparentes:** Puedes ignorar clicks con CSS `pointer-events`.

### Ejemplo 4: Texto Seleccionable y DOM Vivo (AOM)
```svg
<svg width="300" height="80" xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0; background:#f8fafc; display:block;">
  <text x="150" y="45" text-anchor="middle" font-size="20" 
        fill="#1e293b" font-family="sans-serif" font-weight="bold">
    Selecciona este texto 👆
  </text>
</svg>
```

### Ejemplo 5: Hit-Testing (La caja no importa, el relleno sí)
El evento click sólo será válido si se interactúa con los pixeles matemáticos 
del trazo (si hubiese JS, solo el círculo dispara el evento).
```svg
<svg width="150" height="100" xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0; background:#f8fafc; display:block;">
  <!-- bounding box rect invisible para depurar: -->
  <rect x="45" y="20" width="60" height="60" stroke="#ef4444" fill="none" 
        stroke-dasharray="2,2"/>
  <!-- El vector físico: -->
  <circle cx="75" cy="50" r="30" fill="#3b82f6" style="cursor: pointer;"/>
</svg>
```

---

## CSS y la Especificidad de los Presentation Attributes

Puedes estilar SVG con CSS. Los colores o contornos a menudo se definen con 
**Presentation Attributes** (`fill`, `stroke`). 

Dato avanzado: **Un Presentation Attribute tiene la especificidad CSS más baja 
posible**. Si tu SVG incluye `fill="red"` y un CSS externo declara 
`circle { fill: blue; }`, ganará el azul. Esto permite tematizar componentes.

### Ejemplo 6: Tema CSS anulando Presentation Attribute
```svg
<svg width="150" height="80" xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0; background:#f8fafc; display:block;">
  <style>
    /* CSS externo anula el Presentation Attribute (fill="blue") */
    .theme-red { fill: #ef4444; }
  </style>
  <circle cx="75" cy="40" r="20" fill="#3b82f6" class="theme-red"/>
</svg>
```

### Ejemplo 7: Tema Alternativo (Dark Mode)
```svg
<svg width="150" height="80" xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0; background:#f8fafc; display:block;">
  <style>
    /* Mismo SVG reutilizado para estado Dark */
    .theme-dark { fill: #1e293b; }
  </style>
  <circle cx="75" cy="40" r="20" fill="#3b82f6" class="theme-dark"/>
</svg>
```

### Ejemplo 8: Interacción CSS Hover nativa
```svg
<svg width="150" height="80" xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0; background:#f8fafc; display:block;">
  <style>
    /* CSS :hover sobre un nodo SVG */
    .btn:hover { fill: #10b981; transition: fill 0.3s ease; }
  </style>
  <circle cx="75" cy="40" r="20" fill="#3b82f6" class="btn" 
          style="cursor:pointer;"/>
</svg>
```

---

## Versionado Git y Compresión de Red (Brotli)

Al ser puro texto XML, puedes usar `git diff` para revisar si un elemento 
cambió de propiedades, evaluándolo nodo por nodo (imposible con un PNG).

Además, al ser diccionarios repetitivos (la palabra `path`, `fill`, etc.), 
algoritmos en el servidor como **Brotli** o **GZip** pueden reducir un SVG
masivo de 50 KB a solo 4 KB de transferencia en la red.

### Ejemplo 9: Auditable por Git Diff
```svg
<svg width="300" height="100" xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0; background:#0f172a; display:block;">
  <text x="16" y="30" font-family="monospace" font-size="12" fill="#6a9955">
    # git diff --stat ui/avatar.svg
  </text>
  <text x="16" y="55" font-family="monospace" font-size="12" fill="#ef4444">
    - &lt;circle fill="red" r="20"/&gt;
  </text>
  <text x="16" y="80" font-family="monospace" font-size="12" fill="#10b981">
    + &lt;circle fill="blue" r="20"/&gt;
  </text>
</svg>
```

### Ejemplo 10: Optimización de Diccionarios Textuales
Observa aquí cuántas veces se repite el string `x="10"` o `<rect>`. Brotli
detectará que este nodo es repetitivo y comprimirá los 5 rectángulos como si 
sólo estuvieran ocupando el peso de 1. ¡Esta es la magia!
```svg
<svg width="300" height="120" xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0; background:#f8fafc; display:block;">
  <!-- Servidor web Gzip / Brotli comprimirá estos nodos similares -->
  <rect x="20" y="20" width="40" height="80" fill="#94a3b8" />
  <rect x="70" y="30" width="40" height="70" fill="#64748b" />
  <rect x="120" y="40" width="40" height="60" fill="#475569" />
  <rect x="170" y="50" width="40" height="50" fill="#334155" />
  <rect x="220" y="60" width="40" height="40" fill="#1e293b" />
</svg>
```
