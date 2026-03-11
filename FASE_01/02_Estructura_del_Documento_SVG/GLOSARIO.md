# Glosario visual — Sección 02: Estructura del Documento SVG

---

## viewport

El área física que ocupa el SVG en pantalla. Se define con `width` y `height` en el elemento `<svg>`.

```svg
<svg width="260" height="90"
     xmlns="http://www.w3.org/2000/svg"
     style="border:3px solid #3b82f6;background:#f8fafc;display:block; font-family:sans-serif;">
  <!-- El borde azul ES el viewport: 260×90 px en pantalla -->
  <text x="130" y="42" text-anchor="middle"
        font-size="11" fill="#1e40af" font-weight="600">
    viewport = 260 × 90 px
  </text>
  <text x="130" y="60" text-anchor="middle"
        font-size="9" fill="#64748b">
    definido por width="260" height="90" en &lt;svg&gt;
  </text>
</svg>
```

---

## `<defs>`

Contenedor de definiciones. Los elementos dentro no se renderizan directamente — solo cuando son referenciados por `id`.

```svg
<svg width="260" height="110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <!-- Invisible hasta ser referenciado -->
    <linearGradient id="glosGrad" x1="0" y1="0" x2="1" y2="0">
      <stop offset="0%" stop-color="#3b82f6"/>
      <stop offset="100%" stop-color="#10b981"/>
    </linearGradient>
  </defs>

  <!-- &lt;defs&gt; contiene esto ↓ pero no se ve aquí -->
  <rect x="10" y="10" width="240" height="30" rx="4"
        fill="#f1f5f9" stroke="#e2e8f0" stroke-width="1"/>
  <text x="130" y="30" text-anchor="middle"
        font-size="8" fill="#94a3b8" font-family="monospace">
    ← dentro de &lt;defs&gt; (invisible)
  </text>

  <!-- Referenciado aquí: aparece -->
  <rect x="10" y="50" width="240" height="40" rx="6"
        fill="url(#glosGrad)"/>
  <text x="130" y="75" text-anchor="middle"
        font-size="10" fill="white" font-weight="600">
    fill="url(#glosGrad)" → visible ✓
  </text>
</svg>
```

---

## Modelo del pintor (Painter's Model)

El algoritmo de renderizado SVG: cada elemento se dibuja encima de los anteriores. Primero en el código = al fondo.

```svg
<svg width="260" height="130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- 1° → fondo -->
  <rect x="10" y="30" width="240" height="90" rx="8" fill="#1e40af"/>
  <!-- 2° → encima del rect -->
  <ellipse cx="130" cy="75" rx="70" ry="35" fill="#3b82f6"/>
  <!-- 3° → encima de todo → frente -->
  <text x="130" y="80" text-anchor="middle"
        font-size="14" fill="white" font-weight="700">
    Yo soy el 3° → frente
  </text>

  <text x="130" y="22" text-anchor="middle"
        font-size="9" fill="#64748b">
    Orden: ① rect ② ellipse ③ text
  </text>
</svg>
```

---

## `<title>` (en SVG)

Texto alternativo del SVG, equivalente al `alt` de `<img>`. Lo anuncia el lector de pantalla. No se muestra visualmente.

```svg
<svg width="260" height="90"
     role="img" aria-labelledby="glos-title"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <title id="glos-title">Gráfico: crecimiento de ventas</title>

  <!-- Representación visual de "what the title says" -->
  <text x="130" y="25" text-anchor="middle"
        font-size="9" fill="#64748b">
    &lt;title&gt; (invisible para humanos, visible para AT)
  </text>
  <rect x="30" y="34" width="200" height="28" rx="4"
        fill="#1e293b" opacity="0.08"/>
  <text x="130" y="53" text-anchor="middle"
        font-size="9" fill="#475569" font-family="monospace">
    "Gráfico: crecimiento de ventas"
  </text>
  <text x="130" y="78" text-anchor="middle"
        font-size="8" fill="#94a3b8">
    El lector de pantalla anuncia este texto
  </text>
</svg>
```

---

## prólogo XML

La instrucción `<?xml ...?>` que puede aparecer al inicio de un archivo `.svg` externo. No es un elemento XML — es una instrucción de procesamiento.

```svg
<svg width="260" height="80"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#0f172a;display:block; font-family:monospace;">

  <text x="16" y="26" font-size="9" fill="#56b6c2">
    &lt;?xml version="1.0" encoding="UTF-8"?&gt;
  </text>
  <text x="16" y="46" font-size="8" fill="#475569">
    ↑ instrucción de procesamiento (no es un tag XML)
  </text>
  <text x="16" y="62" font-size="8" fill="#475569">
    Solo en archivos .svg — nunca en SVG inline en HTML
  </text>
</svg>
```

---

## aria-hidden / role="img"

Atributos ARIA que controlan cómo los lectores de pantalla interactúan con el SVG.

```svg
<svg width="260" height="110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Decorativo: ocultarlo de AT -->
  <rect x="10" y="10" width="240" height="40" rx="6"
        fill="#fef2f2" stroke="#fecaca" stroke-width="1"/>
  <text x="20" y="28" font-size="8" fill="#991b1b" font-family="monospace">
    aria-hidden="true"
  </text>
  <text x="20" y="44" font-size="8" fill="#64748b">
    SVG decorativo — AT lo ignoran completamente
  </text>

  <!-- Informativo: exponerlo como imagen -->
  <rect x="10" y="60" width="240" height="40" rx="6"
        fill="#eff6ff" stroke="#bfdbfe" stroke-width="1"/>
  <text x="20" y="78" font-size="8" fill="#1e40af" font-family="monospace">
    role="img" aria-labelledby="id"
  </text>
  <text x="20" y="92" font-size="8" fill="#64748b">
    SVG informativo — AT lo leen con &lt;title&gt;
  </text>
</svg>
```
