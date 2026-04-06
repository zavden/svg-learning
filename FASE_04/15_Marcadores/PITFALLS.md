# Errores comunes — Marcadores

---

## 1. `refX` mal calculado: el trazo asoma por la flecha

Si `refX` apunta al centro del marker en lugar de a su extremo, el trazo de la línea llega hasta el centro y el pico de la flecha asoma por delante. La solución es poner `refX` en el mismo punto del marker que debe "tocar" el vértice (normalmente el extremo derecho del triángulo).

```svg
<svg width="300" height="190"
     viewBox="0 0 300 190"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <marker id="mal-refx" viewBox="0 0 10 10" refX="5" refY="5"
            markerWidth="10" markerHeight="10" orient="auto">
      <path d="M 0 0 L 10 5 L 0 10 z" fill="#ef4444"/>
    </marker>
    <marker id="bien-refx" viewBox="0 0 10 10" refX="10" refY="5"
            markerWidth="10" markerHeight="10" orient="auto">
      <path d="M 0 0 L 10 5 L 0 10 z" fill="#10b981"/>
    </marker>
  </defs>

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">refX debe apuntar al extremo "activo"</text>

  <rect x="15" y="35" width="130" height="140" fill="#fef2f2" stroke="#ef4444" stroke-width="1.5" rx="4"/>
  <text x="80" y="52" text-anchor="middle" font-size="9" fill="#9d174d" font-weight="bold">MAL</text>
  <text x="80" y="66" text-anchor="middle" font-size="7" fill="#64748b">refX=5 (centro)</text>
  <line x1="25" y1="110" x2="130" y2="110" stroke="#1e293b" stroke-width="5"
        marker-end="url(#mal-refx)"/>
  <text x="80" y="155" text-anchor="middle" font-size="7" fill="#ef4444">el trazo asoma</text>

  <rect x="155" y="35" width="130" height="140" fill="#dcfce7" stroke="#10b981" stroke-width="1.5" rx="4"/>
  <text x="220" y="52" text-anchor="middle" font-size="9" fill="#166534" font-weight="bold">BIEN</text>
  <text x="220" y="66" text-anchor="middle" font-size="7" fill="#64748b">refX=10 (pico)</text>
  <line x1="165" y1="110" x2="270" y2="110" stroke="#1e293b" stroke-width="5"
        marker-end="url(#bien-refx)"/>
  <text x="220" y="155" text-anchor="middle" font-size="7" fill="#166534">trazo limpio</text>
</svg>
```

---

## 2. El `marker-start` apunta en la dirección equivocada

Con `orient="auto"`, el `marker-end` se alinea con la tangente saliente y funciona bien. Pero el `marker-start` toma la **misma orientación** que la tangente de partida, así que si pones una flecha apuntando a la derecha, aparece invertida en el start. La solución es `orient="auto-start-reverse"`.

```svg
<svg width="300" height="200"
     viewBox="0 0 300 200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <marker id="mal-auto" viewBox="0 0 10 10" refX="9" refY="5"
            markerWidth="7" markerHeight="7" orient="auto">
      <path d="M 0 0 L 10 5 L 0 10 z" fill="#ef4444"/>
    </marker>
    <marker id="bien-rev" viewBox="0 0 10 10" refX="9" refY="5"
            markerWidth="7" markerHeight="7" orient="auto-start-reverse">
      <path d="M 0 0 L 10 5 L 0 10 z" fill="#10b981"/>
    </marker>
  </defs>

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">flechas bidireccionales</text>

  <rect x="15" y="35" width="130" height="150" fill="#fef2f2" stroke="#ef4444" stroke-width="1.5" rx="4"/>
  <text x="80" y="52" text-anchor="middle" font-size="9" fill="#9d174d" font-weight="bold">MAL</text>
  <text x="80" y="66" text-anchor="middle" font-size="7" fill="#64748b">orient="auto"</text>
  <line x1="30" y1="115" x2="130" y2="115" stroke="#1e293b" stroke-width="1.5"
        marker-start="url(#mal-auto)" marker-end="url(#mal-auto)"/>
  <text x="80" y="160" text-anchor="middle" font-size="7" fill="#ef4444">el start apunta al interior</text>

  <rect x="155" y="35" width="130" height="150" fill="#dcfce7" stroke="#10b981" stroke-width="1.5" rx="4"/>
  <text x="220" y="52" text-anchor="middle" font-size="9" fill="#166534" font-weight="bold">BIEN</text>
  <text x="220" y="66" text-anchor="middle" font-size="7" fill="#64748b">auto-start-reverse</text>
  <line x1="170" y1="115" x2="270" y2="115" stroke="#1e293b" stroke-width="1.5"
        marker-start="url(#bien-rev)" marker-end="url(#bien-rev)"/>
  <text x="220" y="160" text-anchor="middle" font-size="7" fill="#166534">flechas coherentes</text>
</svg>
```

---

## 3. El marker cambia de tamaño sin quererlo

Por defecto `markerUnits="strokeWidth"`: el tamaño del marker se multiplica por el `stroke-width` del elemento. Si reutilizas el mismo marker en líneas de distinto grosor, cada una verá una flecha distinta. La solución es `markerUnits="userSpaceOnUse"` para tamaño fijo.

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <marker id="u-default" viewBox="0 0 10 10" refX="9" refY="5"
            markerWidth="5" markerHeight="5" orient="auto">
      <path d="M 0 0 L 10 5 L 0 10 z" fill="#ef4444"/>
    </marker>
    <marker id="u-fixed" viewBox="0 0 10 10" refX="9" refY="5"
            markerWidth="10" markerHeight="10"
            markerUnits="userSpaceOnUse" orient="auto">
      <path d="M 0 0 L 10 5 L 0 10 z" fill="#10b981"/>
    </marker>
  </defs>

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">el marker escala con el stroke</text>

  <rect x="15" y="35" width="130" height="170" fill="#fef2f2" stroke="#ef4444" stroke-width="1.5" rx="4"/>
  <text x="80" y="52" text-anchor="middle" font-size="9" fill="#9d174d" font-weight="bold">MAL</text>
  <text x="80" y="64" text-anchor="middle" font-size="7" fill="#64748b">default (strokeWidth)</text>
  <line x1="25" y1="85"  x2="130" y2="85"  stroke="#1e293b" stroke-width="1" marker-end="url(#u-default)"/>
  <line x1="25" y1="115" x2="130" y2="115" stroke="#1e293b" stroke-width="3" marker-end="url(#u-default)"/>
  <line x1="25" y1="150" x2="130" y2="150" stroke="#1e293b" stroke-width="6" marker-end="url(#u-default)"/>
  <text x="80" y="190" text-anchor="middle" font-size="7" fill="#ef4444">cada flecha escala distinto</text>

  <rect x="155" y="35" width="130" height="170" fill="#dcfce7" stroke="#10b981" stroke-width="1.5" rx="4"/>
  <text x="220" y="52" text-anchor="middle" font-size="9" fill="#166534" font-weight="bold">BIEN</text>
  <text x="220" y="64" text-anchor="middle" font-size="7" fill="#64748b">userSpaceOnUse</text>
  <line x1="165" y1="85"  x2="270" y2="85"  stroke="#1e293b" stroke-width="1" marker-end="url(#u-fixed)"/>
  <line x1="165" y1="115" x2="270" y2="115" stroke="#1e293b" stroke-width="3" marker-end="url(#u-fixed)"/>
  <line x1="165" y1="150" x2="270" y2="150" stroke="#1e293b" stroke-width="6" marker-end="url(#u-fixed)"/>
  <text x="220" y="190" text-anchor="middle" font-size="7" fill="#166534">tamaño consistente</text>
</svg>
```

---

## 4. El marker hereda un color que no esperabas

El fill del marker está definido dentro del `<marker>`, así que **no** hereda del stroke de la línea. Si defines un marker con `fill="black"` y lo aplicas a una línea roja, la flecha saldrá negra. Solución: `fill="context-stroke"` (SVG 2) o `fill="currentColor"` con `color` CSS en el padre.

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <marker id="col-fixed" viewBox="0 0 10 10" refX="9" refY="5"
            markerWidth="7" markerHeight="7" orient="auto">
      <path d="M 0 0 L 10 5 L 0 10 z" fill="#1e293b"/>
    </marker>
    <marker id="col-ctx" viewBox="0 0 10 10" refX="9" refY="5"
            markerWidth="7" markerHeight="7" orient="auto">
      <path d="M 0 0 L 10 5 L 0 10 z" fill="context-stroke"/>
    </marker>
  </defs>

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">el marker no hereda color del trazo</text>

  <rect x="15" y="35" width="130" height="170" fill="#fef2f2" stroke="#ef4444" stroke-width="1.5" rx="4"/>
  <text x="80" y="52" text-anchor="middle" font-size="9" fill="#9d174d" font-weight="bold">MAL</text>
  <text x="80" y="66" text-anchor="middle" font-size="7" fill="#64748b">fill="#1e293b" fijo</text>
  <line x1="25" y1="90"  x2="130" y2="90"  stroke="#ef4444" stroke-width="2" marker-end="url(#col-fixed)"/>
  <line x1="25" y1="115" x2="130" y2="115" stroke="#10b981" stroke-width="2" marker-end="url(#col-fixed)"/>
  <line x1="25" y1="140" x2="130" y2="140" stroke="#f59e0b" stroke-width="2" marker-end="url(#col-fixed)"/>
  <text x="80" y="180" text-anchor="middle" font-size="7" fill="#ef4444">puntas siempre oscuras</text>

  <rect x="155" y="35" width="130" height="170" fill="#dcfce7" stroke="#10b981" stroke-width="1.5" rx="4"/>
  <text x="220" y="52" text-anchor="middle" font-size="9" fill="#166534" font-weight="bold">BIEN</text>
  <text x="220" y="66" text-anchor="middle" font-size="7" fill="#64748b">fill="context-stroke"</text>
  <line x1="165" y1="90"  x2="270" y2="90"  stroke="#ef4444" stroke-width="2" marker-end="url(#col-ctx)"/>
  <line x1="165" y1="115" x2="270" y2="115" stroke="#10b981" stroke-width="2" marker-end="url(#col-ctx)"/>
  <line x1="165" y1="140" x2="270" y2="140" stroke="#f59e0b" stroke-width="2" marker-end="url(#col-ctx)"/>
  <text x="220" y="180" text-anchor="middle" font-size="7" fill="#166534">cada flecha hereda</text>
</svg>
```

---

## 5. Esperar `marker-mid` en puntos intermedios de una curva

`marker-mid` solo aparece en los **anclas explícitas** del path: los puntos donde empieza un nuevo segmento `L`, `M`, `C`, `Q`, etc. En una curva `Q` no hay markers entre los dos extremos, aunque geométricamente haya una curva larga. Para distribuir símbolos a lo largo de una curva, necesitas JS con `getPointAtLength()` o dividir el path en muchos segmentos.

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <marker id="mid-wait" viewBox="0 0 10 10" refX="5" refY="5"
            markerWidth="5" markerHeight="5">
      <circle cx="5" cy="5" r="4" fill="#f59e0b"/>
    </marker>
  </defs>

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">marker-mid solo en vértices explícitos</text>

  <!-- Esperado -->
  <rect x="15" y="35" width="130" height="160" fill="#fef2f2" stroke="#ef4444" stroke-width="1.5" rx="4"/>
  <text x="80" y="52" text-anchor="middle" font-size="9" fill="#9d174d" font-weight="bold">ESPERADO</text>
  <text x="80" y="64" text-anchor="middle" font-size="6" fill="#64748b">dots a lo largo de la curva</text>
  <path d="M 25 115 Q 80 65 135 115" fill="none" stroke="#64748b" stroke-width="1.5"/>
  <circle cx="35" cy="100" r="3" fill="#f59e0b" opacity="0.4"/>
  <circle cx="55" cy="85" r="3" fill="#f59e0b" opacity="0.4"/>
  <circle cx="80" cy="78" r="3" fill="#f59e0b" opacity="0.4"/>
  <circle cx="105" cy="85" r="3" fill="#f59e0b" opacity="0.4"/>
  <circle cx="125" cy="100" r="3" fill="#f59e0b" opacity="0.4"/>
  <text x="80" y="180" text-anchor="middle" font-size="6" fill="#ef4444">lo que (quizás) querías</text>

  <!-- Real -->
  <rect x="155" y="35" width="130" height="160" fill="#dcfce7" stroke="#10b981" stroke-width="1.5" rx="4"/>
  <text x="220" y="52" text-anchor="middle" font-size="9" fill="#166534" font-weight="bold">REAL</text>
  <text x="220" y="64" text-anchor="middle" font-size="6" fill="#64748b">marker solo en los anclas</text>
  <path d="M 165 115 Q 220 65 275 115"
        fill="none" stroke="#64748b" stroke-width="1.5"
        marker-start="url(#mid-wait)"
        marker-mid="url(#mid-wait)"
        marker-end="url(#mid-wait)"/>
  <text x="220" y="180" text-anchor="middle" font-size="6" fill="#166534">solo 2 puntos (inicio y fin)</text>
</svg>
```
