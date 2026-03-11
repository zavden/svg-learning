# Challenge 04 — SVG responsivo con relación de aspecto

**Tema:** viewBox, SVG responsivo, relación de aspecto
**Dificultad:** ⭐⭐☆☆

---

## Tarea

Diseña un **banner de cabecera** responsivo para una web. El banner debe:

1. Ocupar el **100% del ancho** de su contenedor (usando CSS)
2. Mantener una **relación de aspecto 16:9** (como una pantalla de TV o YouTube)
3. Tener contenido que se diseñó en un espacio de **1600×900 unidades**
4. Usar `viewBox` para que el diseño se escale sin deformarse

**Bonus:** ¿Puedes añadir un elemento que quede centrado exactamente en el banner?

---

## Especificaciones del diseño

El banner debe tener:
- Fondo con gradiente horizontal (de azul oscuro a azul claro)
- Un título grande centrado
- Un subtítulo
- Elementos decorativos (círculos, líneas) en las esquinas

Diseña **todo** con coordenadas en el espacio de 1600×900.

---

## Código base (estructura inicial)

```svg
<svg viewBox="0 0 1600 900"
     xmlns="http://www.w3.org/2000/svg"
     style="width:100%; height:auto; display:block;
            border:2px solid #e2e8f0;">

  <!-- TAREA: Diseña el fondo aquí (sugerencia: usa un rect o defs/linearGradient) -->


  <!-- TAREA: Añade el título centrado (cx del banner = 800, cy = 450) -->


  <!-- TAREA: Añade elementos decorativos en las esquinas -->
  <!-- Esquina superior izquierda: cerca de (0, 0) -->
  <!-- Esquina inferior derecha: cerca de (1600, 900) -->


</svg>
```

---

## Pistas

<details>
<summary>Pista 1 — Gradiente SVG</summary>

Para crear un gradiente horizontal, usa `<defs>` con `<linearGradient>`:

```svg
<defs>
  <linearGradient id="grad" x1="0" y1="0" x2="1" y2="0">
    <stop offset="0%"   stop-color="#1e40af"/>
    <stop offset="100%" stop-color="#0ea5e9"/>
  </linearGradient>
</defs>
<rect width="1600" height="900" fill="url(#grad)"/>
```

</details>

<details>
<summary>Pista 2 — Centro del banner</summary>

El banner tiene 1600×900 unidades. El centro exacto es (800, 450). Para el título principal, usa `text-anchor="middle"` con `x="800"` y `y` ajustado para centrarlo verticalmente (la línea de base del texto, no su centro visual).

</details>

<details>
<summary>Pista 3 — Ratio 16:9</summary>

El viewBox `"0 0 1600 900"` ya establece el ratio 16:9 (1600÷900 ≈ 1.778). Con `width:100%; height:auto;` el navegador calcula la altura automáticamente para mantener ese ratio. No necesitas hacer nada más.

</details>

---

## Resultado esperado

```svg-only
<svg viewBox="0 0 1600 900"
     xmlns="http://www.w3.org/2000/svg"
     style="width:100%; height:auto; display:block;">

  <!-- Fondo con gradiente -->
  <defs>
    <linearGradient id="bgGrad" x1="0" y1="0" x2="1" y2="0">
      <stop offset="0%"   stop-color="#0f172a"/>
      <stop offset="50%"  stop-color="#1e40af"/>
      <stop offset="100%" stop-color="#0369a1"/>
    </linearGradient>
    <linearGradient id="accentGrad" x1="0" y1="0" x2="1" y2="1">
      <stop offset="0%"   stop-color="#3b82f6"/>
      <stop offset="100%" stop-color="#6366f1"/>
    </linearGradient>
  </defs>

  <rect width="1600" height="900" fill="url(#bgGrad)"/>

  <!-- Elementos decorativos esquina superior izquierda -->
  <circle cx="0"   cy="0"   r="300" fill="#3b82f6" opacity="0.08"/>
  <circle cx="0"   cy="0"   r="180" fill="#6366f1" opacity="0.10"/>
  <circle cx="0"   cy="0"   r="80"  fill="#8b5cf6" opacity="0.15"/>

  <!-- Elementos decorativos esquina inferior derecha -->
  <circle cx="1600" cy="900" r="350" fill="#0ea5e9" opacity="0.08"/>
  <circle cx="1600" cy="900" r="200" fill="#3b82f6" opacity="0.10"/>
  <circle cx="1600" cy="900" r="90"  fill="#6366f1" opacity="0.15"/>

  <!-- Línea decorativa horizontal -->
  <line x1="200" y1="580" x2="1400" y2="580"
        stroke="#3b82f6" stroke-width="1.5" opacity="0.3"/>

  <!-- Título principal (centrado en 800, 400) -->
  <text x="800" y="410"
        text-anchor="middle"
        font-size="140"
        font-family="system-ui, sans-serif"
        font-weight="900"
        fill="white"
        letter-spacing="-4">
    Mi Empresa
  </text>

  <!-- Subtítulo -->
  <text x="800" y="530"
        text-anchor="middle"
        font-size="52"
        font-family="system-ui, sans-serif"
        font-weight="300"
        fill="#93c5fd"
        letter-spacing="8">
    SOLUCIONES DIGITALES
  </text>

  <!-- CTA pequeño -->
  <rect x="650" y="610" width="300" height="80" rx="40"
        fill="url(#accentGrad)"/>
  <text x="800" y="660"
        text-anchor="middle"
        font-size="38"
        font-family="system-ui, sans-serif"
        font-weight="600"
        fill="white">
    Contáctanos
  </text>

  <!-- Puntos decorativos -->
  <circle cx="200" cy="200" r="6" fill="#93c5fd" opacity="0.6"/>
  <circle cx="350" cy="150" r="4" fill="#6366f1" opacity="0.6"/>
  <circle cx="280" cy="300" r="3" fill="#8b5cf6" opacity="0.5"/>
  <circle cx="1400" cy="700" r="6" fill="#93c5fd" opacity="0.6"/>
  <circle cx="1250" cy="750" r="4" fill="#6366f1" opacity="0.6"/>
  <circle cx="1320" cy="600" r="3" fill="#8b5cf6" opacity="0.5"/>
</svg>
```
