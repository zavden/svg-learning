# Challenge 01 — Viewport fijo a responsivo

**Tema:** viewport, viewBox, SVG responsivo
**Dificultad:** ⭐☆☆☆

---

## Tarea

Este SVG tiene dimensiones fijas en píxeles. Tu misión: conviértelo en un SVG responsivo que mantenga sus proporciones y llene el ancho disponible sin deformarse.

**Requisitos:**
1. Elimina los atributos `width` y `height` fijos (o usa CSS en su lugar)
2. Añade un `viewBox` apropiado
3. El SVG debe escalar al 100% del ancho de su contenedor
4. Las proporciones deben conservarse (no deformarse)

---

## Código base

```svg
<svg width="300" height="200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block;">

  <!-- Logo de una empresa imaginaria -->
  <rect x="20" y="20" width="260" height="160" rx="12"
        fill="#1e40af"/>

  <circle cx="100" cy="100" r="50"
          fill="#3b82f6" opacity="0.8"/>

  <rect x="160" y="60" width="100" height="80" rx="8"
        fill="#60a5fa" opacity="0.9"/>

  <text x="150" y="175" text-anchor="middle"
        font-size="14" fill="white" font-family="sans-serif"
        font-weight="bold">
    MiEmpresa S.A.
  </text>

</svg>
```

---

## Pistas

<details>
<summary>Pista 1 — ¿qué valores usar en viewBox?</summary>

El viewBox debe coincidir con las dimensiones originales del diseño. Si el SVG original era 300×200, el viewBox lógico es `"0 0 300 200"`.

</details>

<details>
<summary>Pista 2 — ¿cómo hacer que llene el ancho?</summary>

Con `style="width:100%; height:auto; display:block;"` en el elemento `<svg>`. El `height:auto` hace que el navegador calcule la altura según el ratio del viewBox.

</details>

---

## Resultado esperado

```svg-only
<svg viewBox="0 0 300 200"
     xmlns="http://www.w3.org/2000/svg"
     style="width:100%; height:auto; display:block;
            border:2px solid #e2e8f0;background:#f8fafc;">

  <!-- Logo de una empresa imaginaria -->
  <rect x="20" y="20" width="260" height="160" rx="12"
        fill="#1e40af"/>

  <circle cx="100" cy="100" r="50"
          fill="#3b82f6" opacity="0.8"/>

  <rect x="160" y="60" width="100" height="80" rx="8"
        fill="#60a5fa" opacity="0.9"/>

  <text x="150" y="175" text-anchor="middle"
        font-size="14" fill="white" font-family="sans-serif"
        font-weight="bold">
    MiEmpresa S.A.
  </text>

</svg>
```
