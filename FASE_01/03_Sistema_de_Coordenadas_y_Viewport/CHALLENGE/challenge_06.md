# Challenge 06 — Depurar bugs de coordenadas

**Tema:** sistema de coordenadas, errores comunes, debugging
**Dificultad:** ⭐⭐⭐☆

---

## Tarea

Cada uno de los siguientes SVGs tiene **un bug** relacionado con el sistema de coordenadas. Tu misión es:

1. Identificar el problema (sin mirar la solución)
2. Corregirlo con el mínimo cambio posible
3. Comparar el resultado antes/después

---

## Bug 1 — El círculo no aparece

```svg
<svg width="200" height="200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block;">

  <!-- Se quiere un círculo centrado en el SVG -->
  <circle x="100" y="100" r="60" fill="#3b82f6" opacity="0.8"/>

  <text x="100" y="190" text-anchor="middle"
        font-size="11" fill="#ef4444">
    ¿Por qué no aparece el círculo?
  </text>
</svg>
```

<details>
<summary>Solución Bug 1</summary>

`<circle>` no tiene atributos `x`/`y` — esos son para `<rect>`. El círculo usa `cx` y `cy` para su centro.

**Corrección:** cambiar `x="100" y="100"` por `cx="100" cy="100"`.

</details>

---

## Bug 2 — El elemento está fuera de la vista

```svg
<svg width="200" height="200" viewBox="100 100 200 200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block;">

  <!-- Se quiere que este cuadrado aparezca centrado -->
  <rect x="50" y="50" width="100" height="100" rx="10"
        fill="#10b981" opacity="0.8"/>

  <text x="100" y="185" text-anchor="middle"
        font-size="11" fill="#ef4444">
    ¿Dónde está el cuadrado verde?
  </text>
</svg>
```

<details>
<summary>Solución Bug 2</summary>

El `viewBox="100 100 200 200"` hace que el viewport empiece en la coordenada (100, 100) del espacio de usuario. Pero el `<rect>` está en (50, 50), que queda **fuera** del viewBox (antes de x=100 e y=100).

**Opción A:** Cambiar el viewBox a `"0 0 200 200"` para incluir el origen.
**Opción B:** Mover el `<rect>` a `x="150" y="150"` (dentro de la región 100-300).

</details>

---

## Bug 3 — El texto aparece boca abajo (inverted)

```svg
<svg width="200" height="200" viewBox="0 0 200 200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block;">

  <!-- Intento de "girar" el sistema de coordenadas para que Y vaya hacia arriba -->
  <g transform="scale(1, -1)">
    <!-- Se quiere el texto centrado en el SVG -->
    <text x="100" y="100" text-anchor="middle"
          font-size="18" fill="#1e40af" font-family="sans-serif">
      Hola mundo
    </text>
  </g>
</svg>
```

<details>
<summary>Análisis Bug 3</summary>

El `scale(1, -1)` invierte el eje Y. Con esto, y=100 se convierte en y=-100 en coordenadas de pantalla, que está **fuera del viewport** (por encima del borde superior).

Para corregirlo, después de invertir el eje Y, hay que trasladar el contenido hacia abajo. El transform correcto es `translate(0, -200) scale(1, -1)` o simplemente `scale(1, -1) translate(0, -200)`. Pero esto se vuelve complicado.

**Lección:** SVG tiene el Y hacia abajo por diseño. Invertirlo para texto produce resultados confusos. Es mejor adaptar el diseño al sistema de coordenadas SVG.

</details>

---

## Bug 4 — La rotación está en el lugar equivocado

```svg
<svg width="200" height="200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block;">

  <!-- Se quiere rotar este rectángulo sobre su propio centro -->
  <rect x="60" y="80" width="80" height="40" rx="6"
        fill="#a855f7" opacity="0.8"
        transform="rotate(45)"/>

  <text x="100" y="185" text-anchor="middle"
        font-size="10" fill="#ef4444" font-family="sans-serif">
    La rotación no es sobre el centro del rect
  </text>
</svg>
```

<details>
<summary>Solución Bug 4</summary>

`rotate(45)` sin punto de pivote rota sobre el origen (0,0) del SVG. El rectángulo tiene su centro en (100, 100) — el centro del rect es x+width/2=60+40=100, y+height/2=80+20=100.

**Corrección:** usar `rotate(45, 100, 100)` — esto rota 45° alrededor del punto (100, 100), que es el centro del rectángulo.

</details>

---

## Bug 5 — El SVG no mantiene proporciones

```svg
<svg width="300" height="100" viewBox="0 0 100 100"
     preserveAspectRatio="none"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block;">

  <!-- Logo que debe verse sin distorsión -->
  <circle cx="50" cy="50" r="40" fill="#f59e0b"/>
  <text x="50" y="55" text-anchor="middle"
        font-size="20" fill="white" font-family="sans-serif"
        font-weight="bold">
    OK
  </text>
</svg>
```

<details>
<summary>Solución Bug 5</summary>

`preserveAspectRatio="none"` estira el viewBox para llenar exactamente el viewport, deformando el contenido. El viewport es 300×100 pero el viewBox es 100×100 (cuadrado) — esto hace que el círculo aparezca como una elipse.

**Corrección:** quitar el atributo `preserveAspectRatio` (o ponerlo a `"xMidYMid meet"`, que es el valor por defecto). Con el valor por defecto, el círculo mantiene su forma y aparece centrado con franjas grises a los lados (pillarboxing).

</details>

---

## Resultado esperado (todos los bugs corregidos)

**Bug 1 — Corregido: cx/cy en lugar de x/y:**

```svg-only
<svg width="200" height="200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #10b981;background:#f8fafc;display:block;">
  <circle cx="100" cy="100" r="60" fill="#3b82f6" opacity="0.8"/>
  <text x="100" y="180" text-anchor="middle"
        font-size="11" fill="#10b981">
    ✓ cx/cy → círculo visible
  </text>
</svg>
```

**Bug 2 — Corregido: viewBox ajustado al origen:**

```svg-only
<svg width="200" height="200" viewBox="0 0 200 200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #10b981;background:#f8fafc;display:block;">
  <rect x="50" y="50" width="100" height="100" rx="10"
        fill="#10b981" opacity="0.8"/>
  <text x="100" y="175" text-anchor="middle"
        font-size="11" fill="#10b981">
    ✓ viewBox="0 0 200 200" → rect visible
  </text>
</svg>
```

**Bug 4 — Corregido: rotate con punto de pivote:**

```svg-only
<svg width="200" height="200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #10b981;background:#f8fafc;display:block;">
  <rect x="60" y="80" width="80" height="40" rx="6"
        fill="#a855f7" opacity="0.8"
        transform="rotate(45, 100, 100)"/>
  <circle cx="100" cy="100" r="4" fill="tomato"/>
  <text x="100" y="185" text-anchor="middle"
        font-size="10" fill="#10b981" font-family="sans-serif">
    ✓ rotate(45, 100, 100) → pivote en el centro
  </text>
</svg>
```

**Bug 5 — Corregido: preserveAspectRatio por defecto:**

```svg-only
<svg width="300" height="100" viewBox="0 0 100 100"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #10b981;background:#f1f5f9;display:block;">
  <circle cx="50" cy="50" r="40" fill="#f59e0b"/>
  <text x="50" y="55" text-anchor="middle"
        font-size="20" fill="white" font-family="sans-serif"
        font-weight="bold">
    OK
  </text>
</svg>
```
