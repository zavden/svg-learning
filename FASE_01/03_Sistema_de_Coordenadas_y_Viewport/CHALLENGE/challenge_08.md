# Challenge 08 — Conversión de unidades y diseño para impresión

**Tema:** unidades SVG, px vs mm vs cm vs pt, diseño para impresión
**Dificultad:** ⭐⭐⭐⭐

---

## Tarea

Diseña una **tarjeta de visita** lista para impresión. Las tarjetas de visita estándar miden **85mm × 55mm** (formato ISO). Debes usar unidades absolutas (mm) en el SVG para que al guardar como archivo .svg y abrirlo con Inkscape o Illustrator, las dimensiones sean exactamente las correctas para imprimir.

**Requisitos:**
1. Usar `width="85mm" height="55mm"` en el elemento `<svg>`
2. Usar `viewBox="0 0 85 55"` para diseñar en un espacio equivalente (1 unit = 1mm)
3. Respetar márgenes de seguridad de 3mm por todos los lados
4. Incluir: nombre, cargo, empresa, email, teléfono, y un elemento gráfico de marca

**Bonus:** Crea dos versiones (anverso y reverso) de la tarjeta.

---

## Contexto: unidades de impresión en SVG

SVG soporta unidades absolutas para impresión:

| Unidad | Equivalencia | Uso típico |
|---|---|---|
| `mm` | milímetros | diseño editorial, tarjetas |
| `cm` | centímetros | carteles, posters |
| `in` | pulgadas | diseño americano |
| `pt` | puntos tipográficos (1pt = 1/72in) | tipografía |
| `pc` | picas (1pc = 12pt) | composición tipográfica |
| `px` | píxeles CSS (1px = 1/96in) | web |

En un SVG para impresión, los textos suelen medirse en `pt` (puntos), ya que la tipografía tradicional usa esta unidad.

---

## Código base (tarjeta con guías de margen)

```svg
<svg width="85mm" height="55mm" viewBox="0 0 85 55"
     xmlns="http://www.w3.org/2000/svg">

  <!-- Fondo blanco -->
  <rect width="85" height="55" fill="white"/>

  <!-- Área segura (margen 3mm) — QUITAR EN PRODUCCIÓN -->
  <rect x="3" y="3" width="79" height="49"
        fill="none" stroke="#e2e8f0" stroke-width="0.3"
        stroke-dasharray="1,1"/>

  <!-- TAREA: Diseña aquí dentro del área segura (entre 3 y 82mm en X, 3 y 52mm en Y) -->
  <!-- Elemento de marca (izquierda) -->
  <!-- Información de contacto (derecha) -->
  <!-- Separador decorativo -->

</svg>
```

---

## Pistas

<details>
<summary>Pista 1 — viewBox en unidades user vs mm</summary>

Con `width="85mm" height="55mm" viewBox="0 0 85 55"`, cada unidad de usuario equivale exactamente a 1mm. Esto hace que el diseño sea muy intuitivo: si quieres un margen de 3mm, usas `x="3"` y `y="3"`.

</details>

<details>
<summary>Pista 2 — Tipografía para impresión</summary>

Para impresión, los textos pequeños suelen ser de 7-9pt. En nuestro sistema (1 unit = 1mm, y 1pt ≈ 0.353mm), un texto de 8pt equivale a aproximadamente 2.8 unidades de usuario. Sin embargo, en SVG puedes mezclar unidades:

```svg
<text font-size="8pt">...</text>
<!-- o equivalentemente en mm: -->
<text font-size="2.8">...</text>
```

Para el previewer HTML, es más seguro usar unidades de usuario directamente (números sin sufijo).

</details>

<details>
<summary>Pista 3 — Colores para impresión</summary>

Para impresión profesional se usa CMYK, pero SVG solo soporta RGB. Elige colores corporativos que se vean bien en pantalla; al imprimir, el software de impresión los convierte. Para garantizar negros puros, usa `#000000`.

</details>

---

## Resultado esperado — Anverso

```svg-only
<svg width="85mm" height="55mm" viewBox="0 0 85 55"
     xmlns="http://www.w3.org/2000/svg"
     style="display:block; border:1px solid #e2e8f0; box-shadow:0 2px 8px rgba(0,0,0,0.15);">

  <defs>
    <linearGradient id="cardGrad" x1="0" y1="0" x2="0" y2="1">
      <stop offset="0%" stop-color="#1e40af"/>
      <stop offset="100%" stop-color="#1e3a8a"/>
    </linearGradient>
  </defs>

  <!-- Fondo blanco -->
  <rect width="85" height="55" fill="white"/>

  <!-- Franja de color izquierda (marca) -->
  <rect x="0" y="0" width="28" height="55" fill="url(#cardGrad)"/>

  <!-- Elemento gráfico en la franja -->
  <circle cx="14" cy="20" r="8" fill="white" opacity="0.15"/>
  <circle cx="14" cy="20" r="5" fill="white" opacity="0.2"/>
  <text x="14" y="24" text-anchor="middle"
        font-size="10" fill="white" font-family="sans-serif"
        font-weight="900">A</text>

  <!-- Nombre de la empresa en la franja -->
  <text x="14" y="38" text-anchor="middle"
        font-size="3.5" fill="white" font-family="sans-serif"
        font-weight="bold" letter-spacing="0.5">
    ACME
  </text>
  <text x="14" y="43" text-anchor="middle"
        font-size="2.5" fill="#93c5fd" font-family="sans-serif"
        letter-spacing="0.3">
    STUDIO
  </text>

  <!-- Área de información (derecha de la franja) -->
  <!-- Nombre -->
  <text x="33" y="16" font-size="6.5" fill="#0f172a"
        font-family="sans-serif" font-weight="700">
    Ana García López
  </text>

  <!-- Cargo -->
  <text x="33" y="22" font-size="3.5" fill="#3b82f6"
        font-family="sans-serif" font-weight="500" letter-spacing="0.3">
    Diseñadora UX/UI Senior
  </text>

  <!-- Separador -->
  <line x1="33" y1="26" x2="79" y2="26"
        stroke="#e2e8f0" stroke-width="0.5"/>

  <!-- Datos de contacto -->
  <text x="33" y="32" font-size="3" fill="#475569"
        font-family="monospace">
    ana@acmestudio.com
  </text>
  <text x="33" y="37.5" font-size="3" fill="#475569"
        font-family="monospace">
    +34 612 345 678
  </text>
  <text x="33" y="43" font-size="3" fill="#475569"
        font-family="monospace">
    acmestudio.com
  </text>
  <text x="33" y="48.5" font-size="3" fill="#64748b"
        font-family="sans-serif">
    Madrid, España
  </text>

  <!-- Punto decorativo -->
  <circle cx="30" cy="32"   r="0.8" fill="#3b82f6"/>
  <circle cx="30" cy="37.5" r="0.8" fill="#3b82f6"/>
  <circle cx="30" cy="43"   r="0.8" fill="#3b82f6"/>
  <circle cx="30" cy="48.5" r="0.8" fill="#3b82f6"/>
</svg>
```

## Resultado esperado — Reverso

```svg-only
<svg width="85mm" height="55mm" viewBox="0 0 85 55"
     xmlns="http://www.w3.org/2000/svg"
     style="display:block; border:1px solid #e2e8f0; box-shadow:0 2px 8px rgba(0,0,0,0.15); margin-top:12px;">

  <defs>
    <linearGradient id="revGrad" x1="0" y1="0" x2="1" y2="1">
      <stop offset="0%" stop-color="#1e40af"/>
      <stop offset="100%" stop-color="#7c3aed"/>
    </linearGradient>
  </defs>

  <!-- Fondo de marca (reverso) -->
  <rect width="85" height="55" fill="url(#revGrad)"/>

  <!-- Patrón de círculos decorativos -->
  <circle cx="0"  cy="0"  r="30" fill="white" opacity="0.04"/>
  <circle cx="85" cy="55" r="40" fill="white" opacity="0.04"/>
  <circle cx="0"  cy="55" r="20" fill="white" opacity="0.05"/>

  <!-- Logo grande centrado -->
  <circle cx="42.5" cy="24" r="14" fill="white" opacity="0.12"/>
  <circle cx="42.5" cy="24" r="10" fill="white" opacity="0.15"/>
  <text x="42.5" y="29" text-anchor="middle"
        font-size="16" fill="white" font-family="sans-serif"
        font-weight="900">A</text>

  <!-- Nombre empresa -->
  <text x="42.5" y="44" text-anchor="middle"
        font-size="7" fill="white" font-family="sans-serif"
        font-weight="900" letter-spacing="4">
    ACME
  </text>
  <text x="42.5" y="50" text-anchor="middle"
        font-size="3.5" fill="#bfdbfe" font-family="sans-serif"
        letter-spacing="5">
    STUDIO
  </text>

  <!-- Línea decorativa -->
  <line x1="20" y1="38" x2="65" y2="38"
        stroke="white" stroke-width="0.4" opacity="0.4"/>
</svg>
```
