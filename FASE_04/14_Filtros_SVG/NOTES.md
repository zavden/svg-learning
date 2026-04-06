# Notas — Filtros SVG

---

## Compatibilidad

| Característica | Chrome | Firefox | Safari | Edge |
|---|---|---|---|---|
| `<filter>` y primitivas básicas | ✓ | ✓ | ✓ | ✓ |
| `feGaussianBlur` · `feOffset` · `feMerge` | ✓ | ✓ | ✓ | ✓ |
| `feColorMatrix` · `feComponentTransfer` | ✓ | ✓ | ✓ | ✓ |
| `feComposite` · `feFlood` · `feBlend` | ✓ | ✓ | ✓ | ✓ |
| `feMorphology` · `feTurbulence` | ✓ | ✓ | ✓ | ✓ |
| `feDisplacementMap` | ✓ | ✓ | ✓ | ✓ |
| `feConvolveMatrix` | ✓ | ✓ | ✓ | ✓ |
| `feDiffuseLighting` / `feSpecularLighting` | ✓ | ✓ | ✓ | ✓ |
| `feDropShadow` (SVG 2) | ✓ | ✓ | ✓ 15.4+ | ✓ |
| `feImage` externo (CORS) | ✓ | ✓ | parcial | ✓ |
| CSS `filter: blur()` · `drop-shadow()` | ✓ | ✓ | ✓ | ✓ |
| CSS `filter: url(#id)` | ✓ | ✓ | ✓ | ✓ |
| CSS `backdrop-filter` | ✓ | ✓ 103+ | ✓ 18+ | ✓ |
| `BackgroundImage` / `BackgroundAlpha` | ✗ | ✗ | ✗ | ✗ |

`BackgroundImage` nunca se implementó de forma portable — los navegadores modernos lo ignoran. Para componer con el fondo, usa `backdrop-filter` en CSS o recorta manualmente.

---

## Atributo `filter` vs propiedad CSS `filter`

### Atributo SVG
- `filter="url(#mi-filtro)"` directamente en el elemento
- Solo acepta referencias a un `<filter>` del SVG

### Propiedad CSS
- `filter: url(#mi-filtro);` — equivalente al atributo
- **Además** acepta funciones nativas sin necesidad de definir nada:
  - `blur(5px)`
  - `brightness(1.2)` · `contrast(1.5)` · `saturate(2)`
  - `grayscale(1)` · `sepia(0.8)` · `invert(1)` · `hue-rotate(90deg)`
  - `drop-shadow(3px 4px 5px rgba(0,0,0,0.4))`
  - `opacity(0.5)`
- Se pueden encadenar: `filter: blur(2px) brightness(1.3) saturate(1.5);`

La versión CSS es mucho más concisa para efectos simples y se anima bien con `transition`.

---

## `filter` dentro de filter region vs objectBoundingBox

```
<filter id="f"
        filterUnits="objectBoundingBox"    ← cómo interpretar x/y/width/height del filter
        primitiveUnits="userSpaceOnUse"     ← cómo interpretar valores dentro de primitivas
        x="-10%" y="-10%"
        width="120%" height="120%">
  <feGaussianBlur stdDeviation="3"/>
  <feOffset dx="5" dy="5"/>
</filter>
```

- `filterUnits`: gobierna `x`, `y`, `width`, `height` del propio `<filter>`. Default: `objectBoundingBox`.
- `primitiveUnits`: gobierna los valores **dentro** de las primitivas (los `dx`/`dy` de `feOffset`, etc.). Default: `userSpaceOnUse`.

Si cambias `primitiveUnits` a `objectBoundingBox`, los `dx="0.1"` de un `feOffset` significan "10% del ancho del bbox" en lugar de "10 unidades de usuario". Útil para filtros que deben escalar con el elemento.

---

## Rendimiento

### Coste por primitiva (aproximado, de barato a caro)

1. **`feOffset`** — solo reubica pixels, sin procesamiento
2. **`feFlood`** — genera color plano
3. **`feColorMatrix`** — operación por pixel independiente
4. **`feComposite`** — combina dos imágenes
5. **`feMerge`** — apila capas
6. **`feMorphology`** — toca vecinos
7. **`feConvolveMatrix`** — kernel por pixel
8. **`feGaussianBlur`** — grande o con `stdDeviation` alto, puede ser muy costoso
9. **`feDisplacementMap`** — lookup por pixel
10. **`feDiffuseLighting` / `feSpecularLighting`** — cálculos de iluminación por pixel

### Buenas prácticas

- **Limita el `filter region`** al área mínima necesaria: menos pixels que procesar
- **Usa `feDropShadow`** en lugar de la cadena manual cuando solo quieras una sombra — el navegador lo optimiza internamente
- **Evita animar `stdDeviation`** en elementos grandes: el pipeline se re-ejecuta por frame
- **Prefiere CSS `filter: blur()`** para blurs uniformes: suele ir acelerado por GPU
- **Un solo `<filter>` con varias primitivas** es más barato que varios filtros encadenados por elementos anidados
- **Reutiliza filtros**: una misma definición aplicada a 100 elementos es más barata que 100 definiciones

---

## Trucos y patrones no obvios

### Glow (brillo exterior) apilando `feMergeNode`

```svg
<filter id="glow">
  <feGaussianBlur stdDeviation="4" result="b"/>
  <feMerge>
    <feMergeNode in="b"/>
    <feMergeNode in="b"/>
    <feMergeNode in="b"/>
    <feMergeNode in="SourceGraphic"/>
  </feMerge>
</filter>
```

Repetir el mismo `feMergeNode` intensifica el halo sin recalcular el blur.

### Sombra interior (inset shadow)

No existe primitiva específica, pero puede construirse invirtiendo la composición:

```svg
<filter id="inset">
  <feGaussianBlur in="SourceAlpha" stdDeviation="4"/>
  <feOffset dx="2" dy="3" result="o"/>
  <feFlood flood-color="#000" flood-opacity="0.6"/>
  <feComposite in2="o" operator="out"/>
  <feComposite in2="SourceGraphic" operator="in"/>
  <feMerge>
    <feMergeNode in="SourceGraphic"/>
    <feMergeNode/>
  </feMerge>
</filter>
```

Clave: `composite out` para invertir la silueta, luego `composite in` contra el original para recortar al interior.

### Sombras de varias capas (Material Design)

Apila varios `feDropShadow`:

```svg
<filter id="elevation-3" x="-50%" y="-50%" width="200%" height="200%">
  <feDropShadow dx="0" dy="1" stdDeviation="1" flood-opacity="0.2"/>
  <feDropShadow dx="0" dy="3" stdDeviation="4" flood-opacity="0.15"/>
  <feDropShadow dx="0" dy="8" stdDeviation="10" flood-opacity="0.1"/>
</filter>
```

Da un aspecto "flotando" mucho más natural que una sola sombra dura.

### Filtros animados con `<animate>` dentro de la primitiva

```svg
<feGaussianBlur stdDeviation="0">
  <animate attributeName="stdDeviation"
           values="0;5;0" dur="2s" repeatCount="indefinite"/>
</feGaussianBlur>
```

Funciona pero es costoso: cada frame re-ejecuta el blur completo. Preferible para efectos puntuales, no para animaciones continuas.

### Filtros sobre `<image>` (fotografías)

Los filtros SVG se aplican igual a elementos `<image>`. Esto permite aplicar efectos que en CSS requerirían librerías externas:

```svg
<image href="foto.jpg" width="400" height="300" filter="url(#glow-pink)"/>
```

---

## Debugging

- **El filtro no se ve**: comprobar que el `id` del filter coincide con `filter="url(#id)"`. El filtro debe estar definido **antes** en el DOM (o al menos en el mismo SVG).
- **El efecto sale recortado**: ampliar la región del filtro con `x`, `y`, `width`, `height`.
- **La sombra aparece con un tinte raro**: cambiar `in="SourceGraphic"` por `in="SourceAlpha"` en el blur.
- **Las referencias `in="xxx"` no funcionan**: verificar que el `result="xxx"` está definido en una primitiva **anterior** en el código.
- **El elemento original desapareció**: asegurarse de que el `<feMerge>` final incluye un `<feMergeNode in="SourceGraphic"/>`.
- **En Safari se ve distinto**: comprobar si usas `BackgroundImage` (no soportado) o `feImage` externo con CORS.

---

## Alternativas modernas

### CSS `backdrop-filter`

Para efectos de "vidrio esmerilado" sobre el fondo (el clásico efecto iOS):

```css
.glass {
  backdrop-filter: blur(10px) saturate(1.5);
  background: rgba(255,255,255,0.1);
}
```

Más eficiente que simular el blur del fondo con filtros SVG y `BackgroundImage` (que nunca funcionó bien). Soportado en todos los navegadores modernos.

### `filter: drop-shadow()` vs `box-shadow`

- `box-shadow`: sombra del rectángulo del bounding box. Ignora la forma real del elemento.
- `filter: drop-shadow()`: sigue el contorno real (respetando transparencias de SVG, PNG, bordes redondeados complejos).

Para SVG siempre preferir `drop-shadow`. Para HTML plano con bordes rectos, `box-shadow` es más eficiente.

---

## Fragmentos útiles

### Efecto "papel" (textura sutil)

```svg
<filter id="paper" x="0" y="0" width="100%" height="100%">
  <feTurbulence type="fractalNoise" baseFrequency="0.8" numOctaves="2"/>
  <feColorMatrix type="matrix"
                 values="0 0 0 0 0.9  0 0 0 0 0.9  0 0 0 0 0.85  0 0 0 0.1 0"/>
  <feComposite in2="SourceGraphic" operator="in"/>
</filter>
```

### Efecto "liquid" (distorsión orgánica)

```svg
<filter id="liquid">
  <feTurbulence type="fractalNoise" baseFrequency="0.015" numOctaves="2" seed="5"/>
  <feDisplacementMap in="SourceGraphic" scale="8"/>
</filter>
```

### Neón (glow saturado)

```svg
<filter id="neon" x="-50%" y="-50%" width="200%" height="200%">
  <feGaussianBlur stdDeviation="3" result="b"/>
  <feMerge>
    <feMergeNode in="b"/>
    <feMergeNode in="b"/>
    <feMergeNode in="SourceGraphic"/>
  </feMerge>
</filter>
```

### Contorno de un texto sin tocar el path

```svg
<filter id="stroke-out" x="-20%" y="-20%" width="140%" height="140%">
  <feMorphology operator="dilate" radius="2" in="SourceAlpha" result="d"/>
  <feFlood flood-color="#1e293b" result="c"/>
  <feComposite in="c" in2="d" operator="in" result="s"/>
  <feMerge>
    <feMergeNode in="s"/>
    <feMergeNode in="SourceGraphic"/>
  </feMerge>
</filter>
```
