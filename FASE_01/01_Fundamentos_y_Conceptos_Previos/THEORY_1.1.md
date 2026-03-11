# 1.1 Qué es SVG

SVG (Scalable Vector Graphics) es un formato de imagen basado en **XML**: texto plano que describe geometría. Es un estándar del W3C, el mismo organismo que define HTML y CSS.

Un archivo `.svg` no guarda píxeles — guarda **instrucciones matemáticas** que el navegador interpreta para dibujar.

---

## SVG es texto plano

A diferencia de PNG o JPEG, puedes abrir un `.svg` con un editor de texto y leer lo que hay dentro. Este es un SVG real:

```svg
<svg width="220" height="80"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block;">

  <!-- Este círculo es solo texto que el navegador "traduce" a gráfico -->
  <circle cx="40" cy="40" r="28" fill="#3b82f6"/>

  <!-- Este rectángulo también es solo texto -->
  <rect x="90" y="20" width="60" height="40" rx="8" fill="#10b981"/>

  <!-- Y este texto es... texto dentro de texto -->
  <text x="170" y="44" font-size="13" fill="#1e293b" font-family="sans-serif">
    Texto
  </text>
</svg>
```

---

## Anatomía de un elemento SVG

Cada elemento SVG sigue la misma estructura XML: etiqueta + atributos.

```svg
<svg width="300" height="140"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#0f172a;display:block;">

  <!-- Líneas apuntando a las partes de la sintaxis -->

  <!-- Etiqueta de apertura -->
  <text x="20" y="30" font-size="10" fill="#f59e0b" font-family="monospace">
    &lt;circle
  </text>
  <text x="90" y="30" font-size="10" fill="#d19a66" font-family="monospace">
    cx="150"
  </text>
  <text x="155" y="30" font-size="10" fill="#d19a66" font-family="monospace">
    cy="80"
  </text>
  <text x="200" y="30" font-size="10" fill="#d19a66" font-family="monospace">
    r="40"
  </text>
  <text x="237" y="30" font-size="10" fill="#98c379" font-family="monospace">
    fill="#3b82f6"
  </text>

  <!-- Auto-cierre -->
  <text x="20" y="48" font-size="10" fill="#56b6c2" font-family="monospace">
    /&gt;
  </text>

  <!-- El resultado -->
  <circle cx="150" cy="95" r="35" fill="#3b82f6"/>

  <!-- Anotaciones -->
  <text x="20"  y="75" font-size="8" fill="#64748b" font-family="sans-serif">etiqueta</text>
  <text x="88"  y="75" font-size="8" fill="#64748b" font-family="sans-serif">atributos</text>
  <text x="150" y="138" text-anchor="middle" font-size="8" fill="#64748b" font-family="sans-serif">resultado</text>
</svg>
```

---

## El elemento raíz `<svg>`

Todo SVG empieza con el elemento `<svg>`. Dentro de él van todos los demás elementos gráficos. El atributo `xmlns` declara el namespace XML de SVG.

```svg
<svg width="280" height="120"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block;">

  <!-- xmlns="http://www.w3.org/2000/svg" le dice al navegador
       que este documento usa el vocabulario SVG (no otro XML) -->

  <!-- Sin xmlns en archivo .svg externo → el navegador no sabe cómo renderizarlo -->
  <!-- En SVG inline dentro de HTML5 → el navegador lo infiere, se puede omitir -->

  <rect x="10" y="10" width="260" height="100" rx="8"
        fill="none" stroke="#3b82f6" stroke-width="1.5" stroke-dasharray="6,3"/>

  <text x="140" y="58" text-anchor="middle"
        font-size="13" fill="#1e293b" font-family="sans-serif">
    Todo el contenido SVG va aquí dentro
  </text>
  <text x="140" y="76" text-anchor="middle"
        font-size="10" fill="#64748b" font-family="sans-serif">
    Este es el elemento &lt;svg&gt; (raíz)
  </text>
</svg>
```

---

## SVG 1.1 vs SVG 2

La base de este roadmap es **SVG 1.1** (2003, segunda edición 2011), la versión más ampliamente soportada. SVG 2 está en desarrollo y los navegadores implementan partes de él progresivamente.

```svg
<svg width="300" height="130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block;">

  <!-- Columna SVG 1.1 -->
  <rect x="10" y="10" width="130" height="110" rx="8"
        fill="#eff6ff" stroke="#3b82f6" stroke-width="1.5"/>
  <text x="75" y="34" text-anchor="middle"
        font-size="11" fill="#1e40af" font-family="sans-serif" font-weight="bold">
    SVG 1.1
  </text>
  <text x="75" y="52" text-anchor="middle"
        font-size="8.5" fill="#3b82f6" font-family="sans-serif">✓ Soporte total</text>
  <text x="75" y="66" text-anchor="middle"
        font-size="8.5" fill="#3b82f6" font-family="sans-serif">✓ Todos los browsers</text>
  <text x="75" y="80" text-anchor="middle"
        font-size="8.5" fill="#3b82f6" font-family="sans-serif">✓ Estable, en producción</text>
  <text x="75" y="108" text-anchor="middle"
        font-size="8" fill="#64748b" font-family="sans-serif">← este roadmap</text>

  <!-- Columna SVG 2 -->
  <rect x="160" y="10" width="130" height="110" rx="8"
        fill="#fefce8" stroke="#ca8a04" stroke-width="1.5"/>
  <text x="225" y="34" text-anchor="middle"
        font-size="11" fill="#92400e" font-family="sans-serif" font-weight="bold">
    SVG 2
  </text>
  <text x="225" y="52" text-anchor="middle"
        font-size="8.5" fill="#ca8a04" font-family="sans-serif">~ En progreso</text>
  <text x="225" y="66" text-anchor="middle"
        font-size="8.5" fill="#ca8a04" font-family="sans-serif">~ Soporte parcial</text>
  <text x="225" y="80" text-anchor="middle"
        font-size="8.5" fill="#ca8a04" font-family="sans-serif">~ Algunas features nuevas</text>
  <text x="225" y="108" text-anchor="middle"
        font-size="8" fill="#64748b" font-family="sans-serif">se mencionará cuando aplique</text>
</svg>
```
