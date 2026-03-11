# 1.4 Graficos Vectoriales vs Rasterizados

La diferencia entre SVG y PNG/JPEG no se reduce a "uno escala y el otro se
pixela". Lo que cambia es el **modelo de representacion** y, por extension,
toda la cadena tecnica: almacenamiento, compresion, transformaciones, coste de
render, capacidad de interaccion y facilidad de mantenimiento.

Un matiz importante: **la pantalla siempre termina mostrando pixeles**. Incluso
un SVG debe rasterizarse antes de llegar al framebuffer. La ventaja del vector
no consiste en "evitar" los pixeles finales, sino en **aplazar la
rasterizacion hasta el ultimo momento** y partir de geometria parametrica en
lugar de una malla fija de muestras.

| Aspecto | Raster | Vector |
|---|---|---|
| Unidad base | Pixel | Primitiva geometrica |
| Resolucion nativa | Fija | Independiente de resolucion |
| Escalado | Requiere remuestreo | Recalculo geometrico |
| Manipulacion | Global | Por nodo/atributo |
| Caso ideal | Foto, textura, pintura compleja | Icono, diagrama, logo, UI |
| Problema tipico | Pixelacion o blur | Exceso de nodos o paths complejos |

---

## Modelo de representacion

Un grafico raster almacena una **matriz finita de muestras**. Si una imagen es
`800 x 600`, el archivo describe 480,000 posiciones discretas. Cada posicion
guarda informacion de color y, segun el formato, alpha, compresion, perfil de
color y metadatos. El dato importante es que la resolucion queda fijada desde
el origen: no existe geometria interna que el navegador pueda reinterpretar.

Un grafico vectorial almacena **instrucciones**: circulos, rectangulos,
poligonos, curvas Bezier, transformaciones y reglas de pintura. Un circulo no
se guarda como "muchos puntos azules", sino como "centro, radio, relleno y
trazo". Ese modelo es mucho mas cercano a una escena que a una foto.

La consecuencia practica es profunda:

- En raster, el dato principal es la **muestra**.
- En vector, el dato principal es la **intencion geometrica**.
- En raster, escalar significa remuestrear.
- En vector, escalar significa recalcular la escena y rasterizarla de nuevo.

### Ejemplo 1: Raster = cuadricula fija

Cada celda representa una muestra ya decidida. El navegador puede ampliarla,
pero no recuperar detalle que nunca existio.

```svg
<svg width="300" height="110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0; background:#f8fafc; display:block;">
  <text x="16" y="20" font-size="10" fill="#92400e">
    Raster: color por muestra
  </text>
  <g transform="translate(48, 34)">
    <rect x="0" y="0" width="18" height="18" fill="#cbd5e1"/>
    <rect x="18" y="0" width="18" height="18" fill="#60a5fa"/>
    <rect x="36" y="0" width="18" height="18" fill="#3b82f6"/>
    <rect x="54" y="0" width="18" height="18" fill="#60a5fa"/>
    <rect x="0" y="18" width="18" height="18" fill="#60a5fa"/>
    <rect x="18" y="18" width="18" height="18" fill="#1d4ed8"/>
    <rect x="36" y="18" width="18" height="18" fill="#1e3a8a"/>
    <rect x="54" y="18" width="18" height="18" fill="#1d4ed8"/>
    <rect x="0" y="36" width="18" height="18" fill="#3b82f6"/>
    <rect x="18" y="36" width="18" height="18" fill="#1e3a8a"/>
    <rect x="36" y="36" width="18" height="18" fill="#1e3a8a"/>
    <rect x="54" y="36" width="18" height="18" fill="#3b82f6"/>
    <rect x="0" y="54" width="18" height="18" fill="#60a5fa"/>
    <rect x="18" y="54" width="18" height="18" fill="#1d4ed8"/>
    <rect x="36" y="54" width="18" height="18" fill="#3b82f6"/>
    <rect x="54" y="54" width="18" height="18" fill="#60a5fa"/>
    <rect x="0" y="0" width="72" height="72"
          fill="none" stroke="#475569" stroke-width="1"/>
  </g>
  <text x="180" y="58" font-size="9" fill="#64748b">
    4 x 4 muestras utiles
  </text>
  <text x="180" y="72" font-size="9" fill="#64748b">
    el detalle maximo ya esta fijado
  </text>
</svg>
```

### Ejemplo 2: Vector = geometria y estilo

Aqui no se almacenan pixeles. Se almacena una escena: un circulo, una linea y
un pequeno marcador con atributos concretos.

```svg
<svg width="300" height="110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0; background:#f8fafc; display:block;">
  <text x="16" y="20" font-size="10" fill="#1e40af">
    Vector: primitivas y atributos
  </text>
  <circle cx="86" cy="62" r="26" fill="#3b82f6"/>
  <line x1="86" y1="62" x2="126" y2="36"
        stroke="#0f172a" stroke-width="3"/>
  <circle cx="126" cy="36" r="6" fill="#ef4444"/>
  <text x="164" y="48" font-size="9" fill="#475569">
    circle(cx, cy, r)
  </text>
  <text x="164" y="62" font-size="9" fill="#475569">
    line(x1, y1, x2, y2)
  </text>
  <text x="164" y="76" font-size="9" fill="#475569">
    fill, stroke y stroke-width
  </text>
</svg>
```

### Ejemplo 3: Reutilizacion estructural

SVG puede describir una forma una sola vez y reutilizarla muchas veces. Ese
tipo de ahorro semantico no existe en un bitmap plano.

```svg
<svg width="300" height="110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0; background:#f8fafc; display:block;">
  <defs>
    <polygon id="mark"
             points="0,-12 12,0 0,12 -12,0"
             fill="#0ea5e9"/>
  </defs>
  <text x="16" y="20" font-size="10" fill="#0f766e">
    Una definicion, tres instancias
  </text>
  <use href="#mark" x="72" y="60"/>
  <use href="#mark" x="132" y="60"
       transform="rotate(20 132 60)"/>
  <use href="#mark"
       transform="translate(192 60) scale(1.4)"/>
  <text x="16" y="94" font-size="9" fill="#475569">
    defs + use reducen duplicacion estructural
  </text>
</svg>
```

---

## Escalado, remuestreo y nitidez

Cuando un raster se agranda, el navegador debe inventar valores intermedios. El
proceso se llama **remuestreo** o **interpolacion**. Dependiendo del algoritmo
usado, el resultado se ve dentado, borroso o artificialmente suavizado. Ningun
algoritmo puede reconstruir informacion que no estaba en la imagen original.

Cuando un vector se agranda, la escena se recalcula sobre el nuevo espacio de
salida y luego se rasteriza. Por eso un mismo icono SVG puede verse correcto en
16 px, 48 px o 320 px sin generar versiones separadas `@2x` o `@3x`.

Pero hay una precision importante: **vectorial no significa automaticamente
perfecto en cualquier circunstancia**. Todavia intervienen el anti-aliasing, la
densidad de pantalla y el comportamiento del trazo. Si aplicas una
transformacion `scale()`, tambien puedes escalar involuntariamente el grosor
del `stroke`.

### Ejemplo 4: Raster ampliado

La forma sigue siendo reconocible, pero la estructura cuadrada del muestreo ya
es visible.

```svg
<svg width="300" height="120"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0; background:#f8fafc; display:block;">
  <text x="16" y="20" font-size="10" fill="#92400e">
    Raster al 400 por ciento
  </text>
  <g transform="translate(54, 30)">
    <rect x="0" y="20" width="20" height="20" fill="#93c5fd"/>
    <rect x="20" y="0" width="20" height="20" fill="#60a5fa"/>
    <rect x="40" y="0" width="20" height="20" fill="#3b82f6"/>
    <rect x="60" y="20" width="20" height="20" fill="#60a5fa"/>
    <rect x="0" y="40" width="20" height="20" fill="#60a5fa"/>
    <rect x="20" y="20" width="20" height="20" fill="#1d4ed8"/>
    <rect x="40" y="20" width="20" height="20" fill="#1e3a8a"/>
    <rect x="60" y="40" width="20" height="20" fill="#1d4ed8"/>
    <rect x="20" y="60" width="20" height="20" fill="#60a5fa"/>
    <rect x="40" y="60" width="20" height="20" fill="#3b82f6"/>
    <rect x="0" y="0" width="80" height="80"
          fill="none" stroke="#475569" stroke-width="1"/>
  </g>
  <text x="182" y="54" font-size="9" fill="#64748b">
    ampliacion = pixeles mas grandes
  </text>
  <text x="182" y="68" font-size="9" fill="#64748b">
    no aparece detalle nuevo
  </text>
</svg>
```

### Ejemplo 5: Vector ampliado

La curva sigue siendo una curva; el renderer solo cambia la resolucion final de
la rasterizacion.

```svg
<svg width="300" height="120"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0; background:#f8fafc; display:block;">
  <text x="16" y="20" font-size="10" fill="#1e40af">
    Vector al mismo tamano final
  </text>
  <circle cx="94" cy="66" r="38" fill="#3b82f6"/>
  <circle cx="94" cy="66" r="38"
          fill="none" stroke="#1e3a8a" stroke-width="2"/>
  <text x="164" y="54" font-size="9" fill="#475569">
    el borde se recalcula
  </text>
  <text x="164" y="68" font-size="9" fill="#475569">
    no depende de una malla previa
  </text>
</svg>
```

### Ejemplo 6: Escalar geometria no siempre debe escalar el trazo

Este matiz es importante en iconografia y diagramas tecnicos. Si quieres que el
contorno conserve grosor visual, debes controlarlo explicitamente.

```svg
<svg width="300" height="130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0; background:#f8fafc; display:block;">
  <text x="16" y="20" font-size="10" fill="#334155">
    scale() con y sin non-scaling-stroke
  </text>
  <g transform="translate(40, 34) scale(1.6)">
    <rect x="0" y="0" width="28" height="28"
          fill="none" stroke="#ef4444" stroke-width="3"/>
  </g>
  <g transform="translate(176, 34) scale(1.6)">
    <rect x="0" y="0" width="28" height="28"
          fill="none" stroke="#10b981" stroke-width="3"
          vector-effect="non-scaling-stroke"/>
  </g>
  <text x="34" y="110" font-size="8" fill="#ef4444">
    stroke tambien escala
  </text>
  <text x="162" y="110" font-size="8" fill="#10b981">
    stroke visual estable
  </text>
</svg>
```

---

## Peso de archivo y coste de renderizado

SVG suele ganar con graficos de baja o media complejidad porque describe una
idea con muy pocos datos: una curva, un relleno, un texto, una transformacion.
Ademas, al ser texto, comprime muy bien con `gzip` o `brotli`, sobre todo si
hay patrones repetidos en atributos o nombres de elementos.

Raster suele ganar cuando la informacion visual es de **alta frecuencia**:
fotografias, ruido, degradados complejos, pinceladas organicas, texturas
realistas o escenas exportadas desde herramientas vectoriales con miles de
nodos. En esos casos, representar todo como DOM y comandos geometricos puede
salir mas caro que enviar una sola textura comprimida.

Tambien hay que separar dos costes distintos:

- **Peso de red**: cuanto ocupa el archivo transferido.
- **Coste de render**: cuanto trabajo exige pintarlo, animarlo y recalcularlo.

Un SVG pequeno puede seguir siendo caro si contiene muchos nodos, filtros o
mascaras. Un PNG mas pesado puede resultar barato para el motor de layout porque
se trata como una sola caja pintable.

### Ejemplo 7: Mucha informacion con pocos nodos

Un grafico simple comunica bien sin inflar el DOM.

```svg
<svg width="300" height="120"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0; background:#f8fafc; display:block;">
  <text x="16" y="20" font-size="10" fill="#0f172a">
    Pocos nodos, alto valor semantico
  </text>
  <line x1="38" y1="90" x2="134" y2="90"
        stroke="#94a3b8" stroke-width="2"/>
  <line x1="38" y1="90" x2="38" y2="34"
        stroke="#94a3b8" stroke-width="2"/>
  <path d="M38 82 L64 68 L88 72 L114 46 L134 54"
        fill="none" stroke="#2563eb" stroke-width="3"/>
  <circle cx="64" cy="68" r="4" fill="#2563eb"/>
  <circle cx="88" cy="72" r="4" fill="#2563eb"/>
  <circle cx="114" cy="46" r="4" fill="#2563eb"/>
  <text x="170" y="58" font-size="9" fill="#475569">
    un path + 3 puntos + 2 ejes
  </text>
  <text x="170" y="72" font-size="9" fill="#475569">
    suficiente para una mini grafica
  </text>
</svg>
```

### Ejemplo 8: Simular textura fotografica con nodos

Esta estrategia escala mal. Cada rectangulo extra suma markup, parseo y coste
de pintura.

```svg
<svg width="300" height="120"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0; background:#f8fafc; display:block;">
  <text x="16" y="20" font-size="10" fill="#7c2d12">
    Muchos nodos para simular una textura
  </text>
  <g transform="translate(34, 34)">
    <rect x="0" y="0" width="16" height="16" fill="#a3a3a3"/>
    <rect x="16" y="0" width="16" height="16" fill="#525252"/>
    <rect x="32" y="0" width="16" height="16" fill="#d6d3d1"/>
    <rect x="48" y="0" width="16" height="16" fill="#44403c"/>
    <rect x="64" y="0" width="16" height="16" fill="#a8a29e"/>
    <rect x="0" y="16" width="16" height="16" fill="#78716c"/>
    <rect x="16" y="16" width="16" height="16" fill="#d4d4d8"/>
    <rect x="32" y="16" width="16" height="16" fill="#57534e"/>
    <rect x="48" y="16" width="16" height="16" fill="#a1a1aa"/>
    <rect x="64" y="16" width="16" height="16" fill="#3f3f46"/>
    <rect x="0" y="32" width="16" height="16" fill="#e7e5e4"/>
    <rect x="16" y="32" width="16" height="16" fill="#737373"/>
    <rect x="32" y="32" width="16" height="16" fill="#d6d3d1"/>
    <rect x="48" y="32" width="16" height="16" fill="#57534e"/>
    <rect x="64" y="32" width="16" height="16" fill="#a8a29e"/>
    <rect x="0" y="48" width="16" height="16" fill="#44403c"/>
    <rect x="16" y="48" width="16" height="16" fill="#d4d4d8"/>
    <rect x="32" y="48" width="16" height="16" fill="#6b7280"/>
    <rect x="48" y="48" width="16" height="16" fill="#e7e5e4"/>
    <rect x="64" y="48" width="16" height="16" fill="#57534e"/>
    <rect x="0" y="0" width="80" height="64"
          fill="none" stroke="#475569" stroke-width="1"/>
  </g>
  <text x="164" y="58" font-size="9" fill="#475569">
    20 rects para un parche minimo
  </text>
  <text x="164" y="72" font-size="9" fill="#475569">
    una foto real pisaria este coste
  </text>
</svg>
```

### Ejemplo 9: El punto de cruce no es fijo

No existe un numero magico universal. Depende de nodos, filtros, animacion,
dispositivo y frecuencia de repintado.

```svg
<svg width="300" height="130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0; background:#f8fafc; display:block;">
  <text x="16" y="20" font-size="10" fill="#334155">
    A mayor complejidad, menor ventaja relativa de SVG
  </text>
  <line x1="34" y1="102" x2="272" y2="102"
        stroke="#cbd5e1" stroke-width="2"/>
  <line x1="34" y1="102" x2="34" y2="32"
        stroke="#cbd5e1" stroke-width="2"/>
  <path d="M34 96 C82 88 120 68 160 52 S230 32 272 26"
        fill="none" stroke="#2563eb" stroke-width="3"/>
  <path d="M34 88 C82 84 136 78 188 72 S244 68 272 66"
        fill="none" stroke="#f59e0b" stroke-width="3"/>
  <circle cx="170" cy="74" r="5" fill="#ef4444"/>
  <text x="236" y="30" font-size="8" fill="#2563eb">
    SVG favorable
  </text>
  <text x="226" y="66" font-size="8" fill="#f59e0b">
    Raster favorable
  </text>
  <text x="142" y="118" font-size="8" fill="#475569">
    complejidad visual y estructural
  </text>
</svg>
```

---

## Implicaciones para CSS, JavaScript y accesibilidad

Una diferencia decisiva para desarrollo web es que un SVG inline puede formar
parte real del DOM. Eso permite:

- seleccionar nodos con CSS,
- cambiar atributos con JavaScript,
- animar partes concretas,
- responder a eventos por elemento,
- exponer nombre y descripcion accesibles.

Un raster embebido en `<img>` es esencialmente una superficie opaca. Puedes
mover, rotar o ocultar la caja completa, pero no recolorear "el tercer circulo"
porque internamente no hay un tercer circulo: solo pixeles.

Tambien conviene distinguir entre **SVG inline** y **SVG externo**:

- `inline`: accesible para CSS/JS del documento.
- `<img src="icon.svg">`: conserva escalado, pero encapsula la escena.
- `background-image`: util para decoracion, pobre para semantica e interaccion.

### Ejemplo 10: CSS actuando sobre partes internas

Este tipo de control fino no existe sobre una imagen raster tradicional.

```svg
<svg width="300" height="120"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0; background:#f8fafc; display:block;">
  <style>
    .frame { fill: #e2e8f0; }
    .lamp { fill: #94a3b8; transition: fill 0.2s ease; }
    .btn:hover .lamp { fill: #f59e0b; }
  </style>
  <text x="16" y="20" font-size="10" fill="#334155">
    Hover en un nodo interno
  </text>
  <g class="btn" transform="translate(92, 34)">
    <rect class="frame" x="0" y="0" width="84" height="40" rx="10"/>
    <circle class="lamp" cx="42" cy="20" r="11"/>
  </g>
  <text x="82" y="96" font-size="9" fill="#475569">
    el hover afecta solo al circulo interno
  </text>
</svg>
```

### Ejemplo 11: Semantica accesible

`title`, `desc` y `aria-labelledby` convierten una figura en un recurso mas
comprensible para tecnologias asistivas.

```svg
<svg width="300" height="120"
     xmlns="http://www.w3.org/2000/svg"
     role="img"
     aria-labelledby="t d"
     style="border:2px solid #e2e8f0; background:#f8fafc; display:block;">
  <title id="t">Indicador de estado correcto</title>
  <desc id="d">
    Circulo verde con marca de verificacion y etiqueta de exito.
  </desc>
  <circle cx="64" cy="60" r="26" fill="#10b981"/>
  <path d="M52 60 L61 69 L78 50"
        fill="none" stroke="white" stroke-width="5"
        stroke-linecap="round" stroke-linejoin="round"/>
  <text x="112" y="58" font-size="10" fill="#0f172a">
    Estado: listo
  </text>
  <text x="112" y="74" font-size="9" fill="#475569">
    nombre y descripcion legibles por AT
  </text>
</svg>
```

### Ejemplo 12: Area de interaccion desacoplada de la forma visible

En SVG puedes crear un objetivo de click mas grande que el dibujo visible. Eso
mejora usabilidad sin deformar el icono.

```svg
<svg width="300" height="120"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0; background:#f8fafc; display:block;">
  <text x="16" y="20" font-size="10" fill="#334155">
    Hit area amplia, icono pequeno
  </text>
  <rect x="74" y="30" width="92" height="60" rx="14"
        fill="#dbeafe" stroke="#93c5fd" stroke-width="2"/>
  <polygon points="110,48 110,72 132,60"
           fill="#1d4ed8"/>
  <text x="178" y="56" font-size="9" fill="#475569">
    la caja puede recibir eventos
  </text>
  <text x="178" y="70" font-size="9" fill="#475569">
    el icono mantiene su tamano optico
  </text>
</svg>
```

### Regla de eleccion rapida

Usa SVG cuando se cumplan varias de estas condiciones:

- la imagen es geometrica, esquematica o iconografica,
- necesitas escalado limpio y una sola fuente para multiples tamanos,
- quieres estilizar partes internas con CSS,
- quieres animar, recolorear o responder a eventos por elemento,
- necesitas semantica o texto accesible dentro del grafico.

Usa raster cuando se cumplan estas otras:

- la imagen es fotografica o muy rica en microdetalle,
- el contenido ya nace como textura o pintura de pixeles,
- no necesitas manipular partes internas,
- el coste de representar miles de formas seria peor que enviar una textura.

Como regla operativa de frontend:

- **SVG** para logos, iconos, diagramas, mapas, UI y data viz moderada.
- **Raster** para fotos, screenshots y texturas organicas.
- **Canvas/WebGL** cuando el problema ya no es solo formato, sino volumen de
  elementos, frecuencia de repintado o visualizacion masiva.
