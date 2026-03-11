# Challenge 03 — Efecto cámara (pan con min-x/min-y)

**Tema:** viewBox min-x y min-y, efecto paneo
**Dificultad:** ⭐⭐☆☆

---

## Tarea

Tienes una ciudad dibujada en un lienzo grande (800×400 unidades). Debes crear **cuatro encuadres de cámara** del mismo dibujo, como si movieras una cámara por la ciudad.

**Requisitos:**
- Los cuatro SVGs tienen el mismo `width="400" height="200"` en pantalla
- Los cuatro SVGs tienen el mismo `viewBox` **ancho y alto** (400×200 unidades)
- Solo cambia el **origen** del viewBox (min-x y min-y) para "mover la cámara"
- Crea encuadres: izquierda, centro-izquierda, centro-derecha, derecha

---

## Código base (ciudad completa — misma para los 4 encuadres)

```svg
<svg width="800" height="400" viewBox="0 0 800 400"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#dbeafe;display:block;">

  <!-- Suelo -->
  <rect x="0" y="300" width="800" height="100" fill="#86efac"/>

  <!-- Edificio 1 (izquierda) -->
  <rect x="30"  y="160" width="80"  height="140" fill="#475569"/>
  <rect x="30"  y="140" width="80"  height="30"  fill="#64748b"/>
  <!-- ventanas -->
  <rect x="45"  y="175" width="18" height="22" fill="#93c5fd"/>
  <rect x="75"  y="175" width="18" height="22" fill="#93c5fd"/>
  <rect x="45"  y="215" width="18" height="22" fill="#93c5fd"/>
  <rect x="75"  y="215" width="18" height="22" fill="#bae6fd"/>
  <rect x="45"  y="255" width="18" height="22" fill="#93c5fd"/>
  <rect x="75"  y="255" width="18" height="22" fill="#93c5fd"/>

  <!-- Edificio 2 (izquierda-centro) -->
  <rect x="150" y="100" width="120" height="200" fill="#334155"/>
  <rect x="150" y="80"  width="120" height="30"  fill="#1e293b"/>
  <rect x="195" y="60"  width="30"  height="30"  fill="#475569"/>
  <!-- ventanas -->
  <rect x="165" y="115" width="25" height="20" fill="#bae6fd"/>
  <rect x="200" y="115" width="25" height="20" fill="#93c5fd"/>
  <rect x="235" y="115" width="25" height="20" fill="#bae6fd"/>
  <rect x="165" y="150" width="25" height="20" fill="#fde68a"/>
  <rect x="200" y="150" width="25" height="20" fill="#93c5fd"/>
  <rect x="235" y="150" width="25" height="20" fill="#93c5fd"/>
  <rect x="165" y="185" width="25" height="20" fill="#93c5fd"/>
  <rect x="200" y="185" width="25" height="20" fill="#bae6fd"/>
  <rect x="235" y="185" width="25" height="20" fill="#fde68a"/>
  <rect x="165" y="220" width="25" height="20" fill="#93c5fd"/>
  <rect x="235" y="220" width="25" height="20" fill="#93c5fd"/>

  <!-- Torre central -->
  <rect x="340" y="60"  width="120" height="240" fill="#1e293b"/>
  <rect x="340" y="40"  width="120" height="30"  fill="#0f172a"/>
  <rect x="390" y="20"  width="20"  height="30"  fill="#334155"/>
  <rect x="398" y="5"   width="4"   height="20"  fill="#ef4444"/>
  <!-- ventanas torre -->
  <rect x="355" y="75"  width="20" height="18" fill="#bae6fd"/>
  <rect x="385" y="75"  width="20" height="18" fill="#bae6fd"/>
  <rect x="415" y="75"  width="20" height="18" fill="#bae6fd"/>
  <rect x="355" y="105" width="20" height="18" fill="#fde68a"/>
  <rect x="385" y="105" width="20" height="18" fill="#93c5fd"/>
  <rect x="415" y="105" width="20" height="18" fill="#bae6fd"/>
  <rect x="355" y="135" width="20" height="18" fill="#93c5fd"/>
  <rect x="385" y="135" width="20" height="18" fill="#fde68a"/>
  <rect x="415" y="135" width="20" height="18" fill="#93c5fd"/>
  <rect x="355" y="165" width="20" height="18" fill="#bae6fd"/>
  <rect x="385" y="165" width="20" height="18" fill="#bae6fd"/>
  <rect x="415" y="165" width="20" height="18" fill="#fde68a"/>
  <rect x="355" y="195" width="20" height="18" fill="#93c5fd"/>
  <rect x="385" y="195" width="20" height="18" fill="#93c5fd"/>
  <rect x="415" y="195" width="20" height="18" fill="#bae6fd"/>
  <rect x="355" y="225" width="20" height="18" fill="#fde68a"/>
  <rect x="385" y="225" width="20" height="18" fill="#bae6fd"/>
  <rect x="415" y="225" width="20" height="18" fill="#93c5fd"/>

  <!-- Edificio 4 (centro-derecha) -->
  <rect x="510" y="130" width="100" height="170" fill="#475569"/>
  <rect x="510" y="110" width="100" height="30"  fill="#334155"/>
  <!-- ventanas -->
  <rect x="525" y="145" width="22" height="18" fill="#93c5fd"/>
  <rect x="560" y="145" width="22" height="18" fill="#bae6fd"/>
  <rect x="525" y="178" width="22" height="18" fill="#fde68a"/>
  <rect x="560" y="178" width="22" height="18" fill="#93c5fd"/>
  <rect x="525" y="211" width="22" height="18" fill="#93c5fd"/>
  <rect x="560" y="211" width="22" height="18" fill="#bae6fd"/>
  <rect x="525" y="244" width="22" height="18" fill="#93c5fd"/>
  <rect x="560" y="244" width="22" height="18" fill="#fde68a"/>

  <!-- Edificio 5 (derecha) -->
  <rect x="660" y="180" width="100" height="120" fill="#64748b"/>
  <rect x="660" y="160" width="100" height="30"  fill="#475569"/>
  <!-- ventanas -->
  <rect x="675" y="195" width="20" height="18" fill="#93c5fd"/>
  <rect x="710" y="195" width="20" height="18" fill="#bae6fd"/>
  <rect x="675" y="228" width="20" height="18" fill="#fde68a"/>
  <rect x="710" y="228" width="20" height="18" fill="#93c5fd"/>
  <rect x="675" y="261" width="20" height="18" fill="#93c5fd"/>
  <rect x="710" y="261" width="20" height="18" fill="#bae6fd"/>

  <!-- Calle -->
  <rect x="0" y="300" width="800" height="15" fill="#94a3b8"/>
  <rect x="0" y="310" width="800" height="5"  fill="#cbd5e1"/>
  <!-- Líneas de calle -->
  <line x1="0" y1="307" x2="800" y2="307" stroke="#e2e8f0" stroke-width="1" stroke-dasharray="30,20"/>

  <!-- Árboles -->
  <rect x="130"  y="270" width="8" height="30" fill="#92400e"/>
  <ellipse cx="134"  cy="258" rx="18" ry="20" fill="#16a34a"/>
  <rect x="490"  y="270" width="8" height="30" fill="#92400e"/>
  <ellipse cx="494"  cy="258" rx="18" ry="20" fill="#16a34a"/>
  <rect x="640"  y="270" width="8" height="30" fill="#92400e"/>
  <ellipse cx="644"  cy="258" rx="15" ry="18" fill="#22c55e"/>

  <!-- Etiqueta info -->
  <text x="400" y="390" text-anchor="middle" font-size="11"
        fill="#475569" font-family="sans-serif">
    Ciudad completa — 800 × 400 unidades
  </text>
</svg>
```

---

## Tu misión

Crea 4 SVGs con `width="400" height="200"` y `viewBox` con ancho=400 y alto=200, cambiando solo `min-x` y `min-y`:

| Encuadre | min-x | min-y | Descripción |
|---|---|---|---|
| Izquierda | 0 | 100 | Primer edificio y árbol |
| Centro-izquierda | 150 | 50 | Edificio alto + base torre |
| Centro | 200 | 0 | Torre central completa |
| Derecha | 400 | 100 | Edificios de la derecha |

---

## Pistas

<details>
<summary>Pista — viewBox con paneo</summary>

Para un "paneo" a x=200, el viewBox es `"200 0 400 200"`. El viewport (400×200 px) muestra las unidades de x=200 hasta x=600. El contenido no se mueve, solo cambia qué parte ves.

</details>

---

## Resultado esperado

**Encuadre: Izquierda**

```svg-only
<svg width="400" height="200" viewBox="0 100 400 200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#dbeafe;display:block;">
  <rect x="0" y="300" width="800" height="100" fill="#86efac"/>
  <rect x="30"  y="160" width="80"  height="140" fill="#475569"/>
  <rect x="30"  y="140" width="80"  height="30"  fill="#64748b"/>
  <rect x="45"  y="175" width="18" height="22" fill="#93c5fd"/>
  <rect x="75"  y="175" width="18" height="22" fill="#93c5fd"/>
  <rect x="45"  y="215" width="18" height="22" fill="#93c5fd"/>
  <rect x="75"  y="215" width="18" height="22" fill="#bae6fd"/>
  <rect x="45"  y="255" width="18" height="22" fill="#93c5fd"/>
  <rect x="75"  y="255" width="18" height="22" fill="#93c5fd"/>
  <rect x="150" y="100" width="120" height="200" fill="#334155"/>
  <rect x="150" y="80"  width="120" height="30"  fill="#1e293b"/>
  <rect x="130"  y="270" width="8" height="30" fill="#92400e"/>
  <ellipse cx="134"  cy="258" rx="18" ry="20" fill="#16a34a"/>
  <rect x="0" y="300" width="800" height="15" fill="#94a3b8"/>
  <rect x="0" y="310" width="800" height="5"  fill="#cbd5e1"/>
</svg>
```

**Encuadre: Torre central**

```svg-only
<svg width="400" height="200" viewBox="200 0 400 200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#dbeafe;display:block;">
  <rect x="0" y="300" width="800" height="100" fill="#86efac"/>
  <rect x="340" y="60"  width="120" height="240" fill="#1e293b"/>
  <rect x="340" y="40"  width="120" height="30"  fill="#0f172a"/>
  <rect x="390" y="20"  width="20"  height="30"  fill="#334155"/>
  <rect x="398" y="5"   width="4"   height="20"  fill="#ef4444"/>
  <rect x="355" y="75"  width="20" height="18" fill="#bae6fd"/>
  <rect x="385" y="75"  width="20" height="18" fill="#bae6fd"/>
  <rect x="415" y="75"  width="20" height="18" fill="#bae6fd"/>
  <rect x="355" y="105" width="20" height="18" fill="#fde68a"/>
  <rect x="385" y="105" width="20" height="18" fill="#93c5fd"/>
  <rect x="415" y="105" width="20" height="18" fill="#bae6fd"/>
  <rect x="355" y="135" width="20" height="18" fill="#93c5fd"/>
  <rect x="385" y="135" width="20" height="18" fill="#fde68a"/>
  <rect x="415" y="135" width="20" height="18" fill="#93c5fd"/>
  <rect x="510" y="130" width="100" height="170" fill="#475569"/>
  <rect x="510" y="110" width="100" height="30"  fill="#334155"/>
  <rect x="490"  y="270" width="8" height="30" fill="#92400e"/>
  <ellipse cx="494"  cy="258" rx="18" ry="20" fill="#16a34a"/>
  <rect x="0" y="300" width="800" height="15" fill="#94a3b8"/>
</svg>
```
