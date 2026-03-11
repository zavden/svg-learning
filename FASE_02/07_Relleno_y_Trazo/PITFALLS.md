# Errores comunes — Relleno y Trazo

---

## 1. Fill negro por defecto — formas invisibles o confusas

Sin `fill` explícito, todas las formas son negras. Sobre fondo blanco parece correcto; sobre fondo oscuro o con overlapping crea problemas.

```svg
<svg width="300" height="90"
     viewBox="0 0 300 90"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#1e293b;display:block; font-family:sans-serif;">

  <!-- Negro sobre fondo oscuro → invisible -->
  <rect x="10" y="20" width="60" height="50"/>
  <circle cx="110" cy="45" r="25"/>
  <text x="80" y="82" text-anchor="middle" font-size="7.5" fill="#ef4444">¿dónde están?</text>
  <text x="80" y="90" text-anchor="middle" font-size="7" fill="#94a3b8">negro sobre oscuro</text>

  <!-- Con fill explícito -->
  <rect x="180" y="20" width="60" height="50" fill="#3b82f6"/>
  <circle cx="270" cy="45" r="25" fill="#10b981"/>
  <text x="225" y="82" text-anchor="middle" font-size="7.5" fill="#64748b">fill explícito ✓</text>
</svg>
```

---

## 2. `stroke` sin `stroke-width` — trazo de 1px puede quedar invisible

En pantallas de alta densidad (retina), un stroke de 1px puede verse muy fino o desaparecer visualmente.

```svg
<svg width="300" height="80"
     viewBox="0 0 300 80"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- stroke=1 (defecto) — puede ser muy fino -->
  <rect x="10" y="15" width="80" height="50" fill="white" stroke="#3b82f6"/>
  <text x="50" y="76" text-anchor="middle" font-size="7.5" fill="#64748b">sw=1 (defecto)</text>

  <!-- Explícito y suficientemente grueso -->
  <rect x="120" y="15" width="80" height="50" fill="white" stroke="#3b82f6" stroke-width="2"/>
  <text x="160" y="76" text-anchor="middle" font-size="7.5" fill="#3b82f6">sw=2 ✓</text>

  <!-- Para iconos: mínimo 1.5 o 2 -->
  <rect x="230" y="15" width="60" height="50" fill="none" stroke="#3b82f6" stroke-width="1.5"/>
  <text x="260" y="76" text-anchor="middle" font-size="7.5" fill="#3b82f6">sw=1.5</text>
</svg>
```

---

## 3. Stroke recortado en el borde del `viewBox`

El stroke se extiende `stroke-width/2` hacia fuera. Un elemento pegado al borde del viewBox tiene parte del stroke recortada.

```svg
<svg width="300" height="90"
     viewBox="0 0 300 90"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- MAL: rect en el borde, stroke recortado -->
  <rect x="5" y="5" width="135" height="75" rx="4" fill="#fef2f2" stroke="#fecaca" stroke-width="1"/>
  <rect x="0" y="5" width="130" height="70" fill="#dbeafe" stroke="#3b82f6" stroke-width="10"/>
  <text x="65" y="85" text-anchor="middle" font-size="7.5" fill="#ef4444">stroke recortado</text>

  <!-- BIEN: margen = stroke-width/2 = 5px -->
  <rect x="160" y="5" width="135" height="75" rx="4" fill="#f0fdf4" stroke="#bbf7d0" stroke-width="1"/>
  <rect x="165" y="10" width="125" height="65" fill="#dbeafe" stroke="#3b82f6" stroke-width="10"/>
  <text x="227" y="85" text-anchor="middle" font-size="7.5" fill="#10b981">margen de 5px ✓</text>
</svg>
```

---

## 4. CSS sobreescribe atributos sin querer

Una regla CSS global (`rect { fill: blue; }`) sobreescribe todos los atributos `fill` de los rectángulos.

```svg
<svg width="300" height="80"
     viewBox="0 0 300 80"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#fef2f2;display:block; font-family:sans-serif;">

  <style>
    /* Esta regla sobreescribe el fill="red" del rect */
    .override-demo { fill: #10b981 !important; }
  </style>

  <!-- El fill="red" es sobreescrito por la clase CSS -->
  <rect x="20" y="15" width="80" height="50" fill="#ef4444" class="override-demo"/>
  <text x="60" y="73" text-anchor="middle" font-size="7.5" fill="#ef4444">fill="red"</text>
  <text x="60" y="82" text-anchor="middle" font-size="7" fill="#64748b">CSS lo sobreescribe</text>

  <!-- style inline gana sobre CSS -->
  <rect x="160" y="15" width="120" height="50" class="override-demo" style="fill: #a855f7;"/>
  <text x="220" y="73" text-anchor="middle" font-size="7.5" fill="#a855f7">style inline gana ✓</text>
</svg>
```

---

## 5. `stroke-linecap` y `stroke-linejoin` mezclados accidentalmente

`linecap` es para **extremos** (inicio/fin de líneas abiertas). `linejoin` es para **esquinas** (uniones entre segmentos). No confundirlos.

```svg
<svg width="300" height="90"
     viewBox="0 0 300 90"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- linecap="round" en polilínea: afecta extremos -->
  <polyline points="15,70 50,20 85,70"
            stroke="#3b82f6" stroke-width="10" fill="none"
            stroke-linecap="round" stroke-linejoin="miter"/>
  <text x="50" y="85" text-anchor="middle" font-size="7" fill="#3b82f6">linecap=round</text>
  <text x="50" y="93" text-anchor="middle" font-size="6.5" fill="#94a3b8">extremos redondeados</text>

  <!-- linejoin="round" en polilínea: afecta esquina del vértice -->
  <polyline points="115,70 150,20 185,70"
            stroke="#10b981" stroke-width="10" fill="none"
            stroke-linecap="butt" stroke-linejoin="round"/>
  <text x="150" y="85" text-anchor="middle" font-size="7" fill="#10b981">linejoin=round</text>
  <text x="150" y="93" text-anchor="middle" font-size="6.5" fill="#94a3b8">esquina redondeada</text>

  <!-- Ambos round: extremos Y esquinas redondeados -->
  <polyline points="215,70 250,20 285,70"
            stroke="#f59e0b" stroke-width="10" fill="none"
            stroke-linecap="round" stroke-linejoin="round"/>
  <text x="250" y="85" text-anchor="middle" font-size="7" fill="#f59e0b">ambos round</text>
</svg>
```
