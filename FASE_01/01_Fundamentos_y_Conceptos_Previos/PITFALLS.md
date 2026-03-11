# Errores comunes — Sección 01: Fundamentos

Los errores más frecuentes al empezar con SVG, especialmente los que vienen de hábitos de HTML.

---

## Error 1: Olvidar cerrar un elemento

HTML es permisivo con elementos no cerrados. SVG (XML) no: el documento completo deja de renderizarse.

**Incorrecto:**
```svg
<svg width="200" height="80"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #ef4444;background:#fef2f2;display:block;">
  <!-- Este rect no está cerrado — en un .svg externo, nada se renderiza -->
  <!-- En inline HTML el navegador intenta recuperarse, pero el resultado es impredecible -->
  <rect x="20" y="20" width="160" height="40" fill="#ef4444">
  <text x="100" y="47" text-anchor="middle" font-size="14" fill="white">
    Error: rect no cerrado
  </text>
</svg>
```

**Correcto:**
```svg
<svg width="200" height="80"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #10b981;background:#f0fdf4;display:block;">
  <rect x="20" y="20" width="160" height="40" rx="6" fill="#10b981"/>
  <text x="100" y="47" text-anchor="middle" font-size="14" fill="white"
        font-family="sans-serif">
    Correcto ✓
  </text>
</svg>
```

---

## Error 2: Atributos sin comillas

En HTML5 `width=100` funciona. En SVG/XML es un error de sintaxis.

**Incorrecto:**
```svg
<svg width="220" height="80"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #ef4444;background:#fef2f2;display:block;">
  <!-- Los atributos del rect de abajo NO tienen comillas — XML inválido -->
  <!-- En el previewer puede verse porque el browser HTML lo intenta parsear -->
  <rect x="20" y="20" width="180" height="40" fill="#ef4444" rx=8/>
  <text x="110" y="45" text-anchor="middle" font-size="11"
        fill="white" font-family="sans-serif">
    rx=8 sin comillas — XML inválido
  </text>
</svg>
```

**Correcto:**
```svg
<svg width="220" height="80"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #10b981;background:#f0fdf4;display:block;">
  <rect x="20" y="20" width="180" height="40" fill="#10b981" rx="8"/>
  <text x="110" y="45" text-anchor="middle" font-size="11"
        fill="white" font-family="sans-serif">
    rx="8" con comillas — correcto ✓
  </text>
</svg>
```

---

## Error 3: Case incorrecto en nombres de elementos

`<lineargradient>` no existe. `<linearGradient>` sí. La diferencia es silenciosa: el browser ignora el elemento desconocido sin error en consola.

**Incorrecto:**
```svg
<svg width="220" height="80"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #ef4444;background:#fef2f2;display:block;">
  <defs>
    <!-- 'lineargradient' en minúsculas — elemento desconocido, ignorado -->
    <lineargradient id="grad" x1="0" y1="0" x2="1" y2="0">
      <stop offset="0%" stop-color="#3b82f6"/>
      <stop offset="100%" stop-color="#8b5cf6"/>
    </lineargradient>
  </defs>
  <!-- El gradiente no existe → rect se rellena de negro -->
  <rect x="20" y="15" width="180" height="50" fill="url(#grad)" rx="6"/>
  <text x="110" y="46" text-anchor="middle" font-size="10"
        fill="#ef4444" font-family="sans-serif">
    lineargradient → negro (elemento ignorado)
  </text>
</svg>
```

**Correcto:**
```svg
<svg width="220" height="80"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #10b981;background:#f0fdf4;display:block;">
  <defs>
    <linearGradient id="grad2" x1="0" y1="0" x2="1" y2="0">
      <stop offset="0%" stop-color="#3b82f6"/>
      <stop offset="100%" stop-color="#8b5cf6"/>
    </linearGradient>
  </defs>
  <rect x="20" y="15" width="180" height="50" fill="url(#grad2)" rx="6"/>
  <text x="110" y="46" text-anchor="middle" font-size="12"
        fill="white" font-family="sans-serif" font-weight="600">
    linearGradient ✓
  </text>
</svg>
```

---

## Error 4: Confundir SVG inline con archivo .svg externo

En SVG inline (dentro de HTML), el namespace `xmlns` es opcional. En un archivo `.svg` externo, es **obligatorio**. Sin él, muchos viewers y programas no lo reconocen como SVG.

**Para archivo .svg externo (siempre incluir xmlns):**
```svg
<svg width="280" height="80"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #10b981;background:#f0fdf4;display:block;">
  <text x="140" y="35" text-anchor="middle"
        font-size="11" fill="#166534" font-family="sans-serif" font-weight="600">
    ✓ xmlns siempre en archivo .svg externo
  </text>
  <text x="140" y="55" text-anchor="middle"
        font-size="9" fill="#64748b" font-family="monospace">
    xmlns="http://www.w3.org/2000/svg"
  </text>
  <text x="140" y="70" text-anchor="middle"
        font-size="8" fill="#94a3b8" font-family="sans-serif">
    Inkscape, Illustrator, img src="" lo necesitan
  </text>
</svg>
```

---

## Error 5: Usar etiquetas HTML dentro de SVG

`<div>`, `<span>`, `<p>` no son elementos SVG. Dentro de `<svg>` solo se pueden usar elementos SVG. El texto se pone con `<text>`, no con `<p>`.

**Incorrecto:**
```svg
<svg width="240" height="80"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #ef4444;background:#fef2f2;display:block;">
  <!-- <p> y <div> no son elementos SVG — serán ignorados -->
  <p>Este texto no aparecerá</p>
  <div style="color:red">Este tampoco</div>
  <!-- Lo que sí aparece: el texto de abajo (si el browser es tolerante) -->
  <text x="120" y="50" text-anchor="middle"
        font-size="10" fill="#ef4444" font-family="sans-serif">
    p y div ignorados — solo aparece este &lt;text&gt;
  </text>
</svg>
```

**Correcto:**
```svg
<svg width="240" height="80"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #10b981;background:#f0fdf4;display:block;">
  <!-- En SVG, el texto va siempre con el elemento <text> -->
  <text x="120" y="40" text-anchor="middle"
        font-size="14" fill="#166534" font-family="sans-serif" font-weight="700">
    Texto en SVG
  </text>
  <text x="120" y="62" text-anchor="middle"
        font-size="10" fill="#64748b" font-family="sans-serif">
    Siempre con &lt;text&gt;, nunca con &lt;p&gt; ✓
  </text>
</svg>
```
