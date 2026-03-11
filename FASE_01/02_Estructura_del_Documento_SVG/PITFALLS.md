# Errores comunes — Sección 02: Estructura del Documento SVG

---

## Error 1: Esperar que `<defs>` renderice algo visible

Los elementos dentro de `<defs>` son invisibles hasta que se referencian. Es el error más común al aprender gradientes o filtros.

**Incorrecto (gradiente definido pero no referenciado):**
```svg
<svg width="220" height="80"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #ef4444;background:#fef2f2;display:block;">
  <defs>
    <linearGradient id="grad" x1="0" y1="0" x2="1" y2="0">
      <stop offset="0%" stop-color="#3b82f6"/>
      <stop offset="100%" stop-color="#8b5cf6"/>
    </linearGradient>
  </defs>
  <!-- El rect usa fill por defecto (negro) porque olvidamos referenciar el gradiente -->
  <rect x="10" y="15" width="200" height="50" rx="6"/>
  <text x="110" y="46" text-anchor="middle" font-size="10"
        fill="white" font-family="sans-serif">¿Dónde está el gradiente?</text>
</svg>
```

**Correcto (gradiente referenciado con url(#id)):**
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
  <rect x="10" y="15" width="200" height="50" rx="6" fill="url(#grad2)"/>
  <text x="110" y="46" text-anchor="middle" font-size="10"
        fill="white" font-family="sans-serif">✓ Gradiente visible</text>
</svg>
```

---

## Error 2: ID duplicados

Los IDs deben ser únicos en todo el SVG. Si hay dos elementos con el mismo ID, `url(#id)` usa el primero y el segundo queda inaccesible.

**Incorrecto:**
```svg
<svg width="260" height="90"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #ef4444;background:#fef2f2;display:block;">
  <defs>
    <!-- DOS gradientes con el mismo id="grad" -->
    <linearGradient id="grad" x1="0" y1="0" x2="1" y2="0">
      <stop offset="0%" stop-color="#ef4444"/>
      <stop offset="100%" stop-color="#f97316"/>
    </linearGradient>
    <linearGradient id="grad" x1="0" y1="0" x2="0" y2="1">
      <stop offset="0%" stop-color="#3b82f6"/>
      <stop offset="100%" stop-color="#8b5cf6"/>
    </linearGradient>
  </defs>
  <!-- ¿Cuál usa? Depende del browser — comportamiento indefinido -->
  <rect x="10" y="15" width="110" height="60" rx="6" fill="url(#grad)"/>
  <rect x="130" y="15" width="110" height="60" rx="6" fill="url(#grad)"/>
  <text x="65"  y="50" text-anchor="middle" font-size="9" fill="white" font-family="sans-serif">id="grad"</text>
  <text x="185" y="50" text-anchor="middle" font-size="9" fill="white" font-family="sans-serif">id="grad"</text>
</svg>
```

**Correcto:**
```svg
<svg width="260" height="90"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #10b981;background:#f0fdf4;display:block;">
  <defs>
    <linearGradient id="gradRojo" x1="0" y1="0" x2="1" y2="0">
      <stop offset="0%" stop-color="#ef4444"/>
      <stop offset="100%" stop-color="#f97316"/>
    </linearGradient>
    <linearGradient id="gradAzul" x1="0" y1="0" x2="0" y2="1">
      <stop offset="0%" stop-color="#3b82f6"/>
      <stop offset="100%" stop-color="#8b5cf6"/>
    </linearGradient>
  </defs>
  <rect x="10" y="15" width="110" height="60" rx="6" fill="url(#gradRojo)"/>
  <rect x="140" y="15" width="110" height="60" rx="6" fill="url(#gradAzul)"/>
  <text x="65"  y="50" text-anchor="middle" font-size="9" fill="white" font-family="sans-serif">gradRojo ✓</text>
  <text x="195" y="50" text-anchor="middle" font-size="9" fill="white" font-family="sans-serif">gradAzul ✓</text>
</svg>
```

---

## Error 3: Asumir que el último elemento dibujado está "al fondo"

El modelo del pintor confunde a los que vienen de CSS con `z-index`. En SVG, el primero en el código está abajo.

**Incorrecto (queremos el texto al frente, pero está primero en el código):**
```svg
<svg width="220" height="100"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #ef4444;background:#fef2f2;display:block;">
  <!-- Texto primero → queda DETRÁS del rectángulo -->
  <text x="110" y="55" text-anchor="middle"
        font-size="16" fill="#1e293b" font-family="sans-serif" font-weight="700">
    Texto tapado
  </text>
  <!-- Rectángulo después → queda ENCIMA del texto -->
  <rect x="10" y="20" width="200" height="60" rx="8" fill="#ef4444" opacity="0.8"/>
</svg>
```

**Correcto (rectángulo primero, texto al frente):**
```svg
<svg width="220" height="100"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #10b981;background:#f0fdf4;display:block;">
  <!-- Rectángulo primero → queda detrás -->
  <rect x="10" y="20" width="200" height="60" rx="8" fill="#10b981"/>
  <!-- Texto después → queda encima, visible -->
  <text x="110" y="55" text-anchor="middle"
        font-size="16" fill="white" font-family="sans-serif" font-weight="700">
    Texto visible ✓
  </text>
</svg>
```

---

## Error 4: Incluir el prólogo XML en SVG inline

La declaración `<?xml ...?>` solo va en archivos `.svg` independientes. En SVG inline dentro de HTML5, el parser HTML la rechaza o la ignora inesperadamente.

**Incorrecto (inline con prólogo XML):**
```svg
<svg width="280" height="80"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #ef4444;background:#fef2f2;display:block; font-family:sans-serif;">
  <text x="140" y="30" text-anchor="middle"
        font-size="10" fill="#991b1b" font-weight="600">
    Supongamos que había &lt;?xml?&gt; antes del &lt;svg&gt;
  </text>
  <text x="140" y="52" text-anchor="middle"
        font-size="9" fill="#ef4444">
    El parser HTML lo rechaza o rompe el documento
  </text>
  <text x="140" y="70" text-anchor="middle"
        font-size="8" fill="#64748b">
    ✗ Solo en archivos .svg externos
  </text>
</svg>
```

**Correcto:**
```svg
<svg width="280" height="80"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #10b981;background:#f0fdf4;display:block; font-family:sans-serif;">
  <text x="140" y="38" text-anchor="middle"
        font-size="10" fill="#166534" font-weight="600">
    SVG inline en HTML5 — sin prólogo XML
  </text>
  <text x="140" y="60" text-anchor="middle"
        font-size="9" fill="#64748b">
    El parser HTML infiere el namespace automáticamente ✓
  </text>
</svg>
```

---

## Error 5: `<title>` en posición incorrecta

`<title>` debe ser el primer hijo del elemento que describe. Si no es el primer hijo, algunos lectores de pantalla no lo asocian correctamente.

**Incorrecto (title al final del SVG):**
```svg
<svg width="280" height="90"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #ef4444;background:#fef2f2;display:block; font-family:sans-serif;">
  <!-- Contenido gráfico primero... -->
  <circle cx="60" cy="45" r="30" fill="#ef4444"/>
  <rect x="110" y="20" width="150" height="50" rx="6" fill="#f97316"/>
  <!-- ...title al final: algunos AT no lo asocian con el SVG -->
  <title>Gráfico de ejemplo</title>
</svg>
```

**Correcto (title como primer hijo):**
```svg
<svg width="280" height="90"
     role="img" aria-labelledby="tit5"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #10b981;background:#f0fdf4;display:block; font-family:sans-serif;">
  <!-- title como PRIMER hijo -->
  <title id="tit5">Gráfico de ejemplo accesible</title>
  <circle cx="60" cy="45" r="30" fill="#10b981"/>
  <rect x="110" y="20" width="150" height="50" rx="6" fill="#3b82f6"/>
</svg>
```
