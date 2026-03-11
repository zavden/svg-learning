# Errores comunes — Sección 03: Sistema de Coordenadas y Viewport

Cada error tiene un ejemplo **incorrecto** seguido del **correcto** con la explicación de qué falla y por qué.

---

## Pitfall 1: viewBox con unidades o valores faltantes

`viewBox` solo acepta **4 números sin unidad** separados por espacios o comas. Cualquier sufijo (`px`, `%`, `em`) o número faltante invalida el atributo completo — el navegador lo ignora silenciosamente.

### ❌ Incorrecto — viewBox con unidades
```svg
<svg width="200" height="120"
     viewBox="0 0 200px 120px"
     style="border:2px solid #ef4444;background:#f8fafc;display:block;">
  <!-- viewBox="0 0 200px 120px" → INVÁLIDO, el navegador lo ignora -->
  <!-- El SVG se renderiza como si no tuviera viewBox -->
  <!-- Resultado: 1 unidad = 1px (no hay escala) -->
  <circle cx="100" cy="60" r="50" fill="#ef4444" opacity="0.8"/>
  <text x="100" y="64" text-anchor="middle"
        font-size="11" fill="white">viewBox ignorado</text>
</svg>
```

### ✅ Correcto — sin unidades
```svg
<svg width="200" height="120"
     viewBox="0 0 100 60"
     style="border:2px solid #10b981;background:#f8fafc;display:block;">
  <!-- viewBox="0 0 100 60" → VÁLIDO: 4 números, sin unidades -->
  <!-- Escala: 200/100=2x → el círculo en r=25 se ve como r=50px -->
  <circle cx="50" cy="30" r="25" fill="#10b981" opacity="0.8"/>
  <text x="50" y="34" text-anchor="middle"
        font-size="6" fill="white">viewBox correcto</text>
</svg>
```

---

## Pitfall 2: preserveAspectRatio es sensible a mayúsculas

`preserveAspectRatio` distingue mayúsculas/minúsculas. La parte del alineamiento **debe** escribirse con la capitalización exacta: `xMidYMid`, no `xmidymid` ni `XMidYMid`.

### ❌ Incorrecto — todo en minúsculas
```svg
<svg width="260" height="100" viewBox="0 0 100 100"
     preserveAspectRatio="xmidymid meet"
     style="border:2px solid #ef4444;background:#e2e8f0;display:block;">
  <!-- preserveAspectRatio="xmidymid meet" → INVÁLIDO por minúsculas -->
  <!-- El navegador usa el valor por defecto (xMidYMid meet) igualmente -->
  <!-- pero en otros contextos puede comportarse distinto o ser ignorado -->
  <rect width="100" height="100" fill="#fef2f2"/>
  <text x="50" y="48" text-anchor="middle" font-size="8" fill="#ef4444">
    xmidymid
  </text>
  <text x="50" y="60" text-anchor="middle" font-size="6" fill="#94a3b8">
    (inválido)
  </text>
</svg>
```

### ✅ Correcto — capitalización exacta
```svg
<svg width="260" height="100" viewBox="0 0 100 100"
     preserveAspectRatio="xMidYMid meet"
     style="border:2px solid #10b981;background:#e2e8f0;display:block;">
  <!-- preserveAspectRatio="xMidYMid meet" → VÁLIDO -->
  <!-- x + Mid + Y + Mid: cada parte capitalizada correctamente -->
  <rect width="100" height="100" fill="#f0fdf4"/>
  <text x="50" y="48" text-anchor="middle" font-size="8" fill="#10b981">
    xMidYMid
  </text>
  <text x="50" y="60" text-anchor="middle" font-size="6" fill="#64748b">
    (válido)
  </text>
</svg>
```

---

## Pitfall 3: viewBox con ancho o alto igual a 0

Un `viewBox` con `width=0` o `height=0` crea una división por cero. El SVG no renderiza nada. Ocurre fácilmente al construir viewBox dinámicamente con valores no inicializados.

### ❌ Incorrecto — dimensión cero
```svg
<svg width="200" height="120"
     viewBox="0 0 0 120"
     style="border:2px solid #ef4444;background:#f8fafc;display:block;">
  <!-- viewBox="0 0 0 120" → ancho=0 causa error de renderizado -->
  <!-- El contenido no se muestra (o comportamiento indefinido) -->
  <rect width="200" height="120" fill="#fee2e2"/>
  <circle cx="100" cy="60" r="50" fill="#ef4444"/>
  <text x="100" y="64" text-anchor="middle"
        font-size="14" fill="white">¿dónde estoy?</text>
</svg>
```

### ✅ Correcto — dimensiones positivas
```svg
<svg width="200" height="120"
     viewBox="0 0 200 120"
     style="border:2px solid #10b981;background:#f8fafc;display:block;">
  <!-- viewBox="0 0 200 120" → ambas dimensiones positivas, funciona -->
  <rect width="200" height="120" fill="#f0fdf4"/>
  <circle cx="100" cy="60" r="50" fill="#10b981" opacity="0.85"/>
  <text x="100" y="64" text-anchor="middle"
        font-size="14" fill="white">visible ✓</text>
</svg>
```

---

## Pitfall 4: Usar `x, y` en `<circle>` en lugar de `cx, cy`

`<circle>` no tiene atributos `x` e `y`. Los atributos correctos son `cx` y `cy` (center-x, center-y). Si usas `x` e `y`, el navegador los ignora y coloca el círculo en (0, 0).

### ❌ Incorrecto — x, y en circle
```svg
<svg width="200" height="150"
     style="border:2px solid #ef4444;background:#f8fafc;display:block;">
  <!-- x="100" y="75" en circle → IGNORADOS -->
  <!-- El círculo aparece en (0,0): esquina superior izquierda -->
  <circle x="100" y="75" r="40" fill="#ef4444" opacity="0.8"/>
  <text x="100" y="120" text-anchor="middle"
        font-size="10" fill="#64748b">¿por qué está en la esquina?</text>
  <!-- Origen para referencia -->
  <circle cx="0" cy="0" r="5" fill="#1e293b"/>
  <text x="5" y="18" font-size="9" fill="#1e293b">(0,0)</text>
</svg>
```

### ✅ Correcto — cx, cy en circle
```svg
<svg width="200" height="150"
     style="border:2px solid #10b981;background:#f8fafc;display:block;">
  <!-- cx="100" cy="75" → el centro del círculo está en (100, 75) -->
  <circle cx="100" cy="75" r="40" fill="#10b981" opacity="0.8"/>
  <circle cx="100" cy="75" r="4" fill="white"/>
  <text x="100" y="72" text-anchor="middle"
        font-size="9" fill="white">centro (100,75)</text>
  <text x="100" y="130" text-anchor="middle"
        font-size="10" fill="#64748b">centrado correctamente ✓</text>
</svg>
```

---

## Pitfall 5: Confundir la dirección del eje Y

En SVG, Y=0 está en la **parte superior** y los valores positivos crecen **hacia abajo**. Quien viene de matemáticas o sistemas de coordenadas geométricos espera el comportamiento contrario.

### ❌ Incorrecto — esperando Y invertido (como matemáticas)
```svg
<svg width="200" height="150"
     style="border:2px solid #ef4444;background:#f8fafc;display:block;">
  <!-- Intención: colocar el círculo en la parte SUPERIOR -->
  <!-- Error: se usa y=120 creyendo que "más positivo = más arriba" -->
  <!-- Resultado: el círculo aparece ABAJO -->
  <circle cx="100" cy="120" r="25" fill="#ef4444" opacity="0.8"/>
  <text x="100" y="124" text-anchor="middle"
        font-size="8" fill="white">cy=120</text>
  <text x="100" y="24" text-anchor="middle"
        font-size="10" fill="#64748b">¿no debería estar aquí arriba?</text>
  <line x1="100" y1="30" x2="100" y2="90"
        stroke="#64748b" stroke-width="1" stroke-dasharray="4,2"/>
  <polygon points="95,92 100,100 105,92" fill="#64748b"/>
</svg>
```

### ✅ Correcto — Y crece hacia abajo en SVG
```svg
<svg width="200" height="150"
     style="border:2px solid #10b981;background:#f8fafc;display:block;">
  <!-- En SVG: y=0 es ARRIBA, y positivo baja -->
  <!-- Para colocar el círculo arriba, usar cy pequeño (ej. cy=30) -->
  <circle cx="100" cy="30" r="25" fill="#10b981" opacity="0.8"/>
  <text x="100" y="34" text-anchor="middle"
        font-size="8" fill="white">cy=30 → arriba ✓</text>
  <!-- Eje Y con flechas para recordar la dirección -->
  <line x1="20" y1="10" x2="20" y2="140"
        stroke="#3b82f6" stroke-width="1.5"/>
  <polygon points="17,138 20,146 23,138" fill="#3b82f6"/>
  <text x="25" y="145" font-size="8" fill="#3b82f6">+Y</text>
  <text x="25" y="18"  font-size="8" fill="#3b82f6">0</text>
</svg>
```

---

## Pitfall 6: Confundir el efecto de min-x y min-y en viewBox

Un error muy común: creer que `viewBox="50 0 200 100"` **añade** 50 unidades de espacio vacío a la izquierda. En realidad, `min-x=50` significa que el área visible **empieza en x=50** — el contenido situado antes de x=50 queda fuera, y todo se desplaza a la izquierda.

### ❌ Expectativa incorrecta
```svg
<svg width="250" height="100" viewBox="50 0 200 100"
     style="border:2px solid #ef4444;background:#f8fafc;display:block;">
  <!-- min-x=50: muchos esperan ver espacio vacío a la IZQUIERDA -->
  <!-- Lo que realmente pasa: el área visible empieza en x=50 -->
  <!-- Todo lo que estaba en x<50 desaparece; el resto se mueve a izq -->
  <rect x="0"  y="20" width="40" height="60"
        fill="#ef4444" opacity="0.5"/>
  <text x="20" y="54" text-anchor="middle"
        font-size="8" fill="#ef4444">x=0..40</text>
  <text x="20" y="64" text-anchor="middle"
        font-size="7" fill="#ef4444">(invisible)</text>
  <rect x="55" y="20" width="80" height="60"
        fill="#3b82f6" opacity="0.7"/>
  <text x="95" y="54" text-anchor="middle"
        font-size="9" fill="white">x=55..135</text>
  <text x="95" y="65" text-anchor="middle"
        font-size="8" fill="white">visible ✓</text>
  <rect x="160" y="20" width="80" height="60"
        fill="#10b981" opacity="0.7"/>
  <text x="200" y="54" text-anchor="middle"
        font-size="9" fill="white">x=160..240</text>
</svg>
```

### ✅ Entendido — min-x desplaza la cámara
```svg
<svg width="250" height="120"
     style="border:2px solid #10b981;background:#f8fafc;display:block;">
  <!-- Para entender viewBox="50 0 200 100": -->
  <!-- piénsalo como una cámara que EMPIEZA a mirar desde x=50 -->
  <!-- Los 250px del viewport muestran las unidades 50..250 del espacio -->

  <!-- Espacio completo de coordenadas (referencia) -->
  <text x="5" y="14" font-size="8" fill="#94a3b8">
    Espacio de coordenadas completo:
  </text>
  <rect x="5"   y="18" width="40"  height="35" fill="#ef4444" opacity="0.4" rx="2"/>
  <rect x="50"  y="18" width="195" height="35" fill="#3b82f6" opacity="0.3" rx="2"/>
  <text x="25"  y="39" text-anchor="middle" font-size="7" fill="#ef4444">
    x=0..49
  </text>
  <text x="147" y="39" text-anchor="middle" font-size="7" fill="#3b82f6">
    x=50..250 (la cámara encuadra esta región)
  </text>

  <!-- Línea de corte -->
  <line x1="50" y1="16" x2="50" y2="56"
        stroke="#f59e0b" stroke-width="2"/>
  <text x="52" y="14" font-size="7" fill="#f59e0b">min-x=50</text>

  <text x="5" y="78" font-size="8" fill="#64748b">
    Resultado (lo que se ve con viewBox="50 0 200 100"):
  </text>
  <rect x="5" y="85" width="240" height="30"
        fill="#dbeafe" rx="2"/>
  <text x="125" y="104" text-anchor="middle" font-size="8" fill="#1d4ed8">
    solo visible la región x=50..250 → se muestra de izq a der
  </text>
</svg>
```

---

## Pitfall 7: SVG responsivo sin viewBox

`width="100%"` hace que el SVG ocupe el ancho del contenedor, pero sin `viewBox` el contenido no escala — las coordenadas siguen siendo píxeles fijos. Un círculo en `cx="150"` puede quedar fuera de un contenedor pequeño.

### ❌ Incorrecto — width 100% sin viewBox
```svg
<svg width="100%" height="120"
     style="border:2px solid #ef4444;background:#f8fafc;display:block;">
  <!-- width="100%" pero sin viewBox: el ancho escala, las coords NO -->
  <!-- Si el contenedor es menor de 280px, el círculo de la derecha -->
  <!-- puede quedar cortado porque cx=250 siempre son 250px fijos -->
  <circle cx="50"  cy="60" r="35" fill="#3b82f6" opacity="0.7"/>
  <circle cx="150" cy="60" r="35" fill="#10b981" opacity="0.7"/>
  <circle cx="250" cy="60" r="35" fill="#ef4444" opacity="0.7"/>
  <text x="5" y="115" font-size="9" fill="#64748b">
    Achica el previewer: el último círculo se corta
  </text>
</svg>
```

### ✅ Correcto — viewBox + CSS
```svg
<svg viewBox="0 0 300 120"
     style="width:100%;height:auto;border:2px solid #10b981;
            background:#f8fafc;display:block;">
  <!-- viewBox="0 0 300 120" + CSS width:100% -->
  <!-- Ahora las coords 0-300 siempre cabrán en el contenedor -->
  <!-- El SVG escala proporcional: los 3 círculos siempre son visibles -->
  <circle cx="50"  cy="60" r="35" fill="#3b82f6" opacity="0.7"/>
  <circle cx="150" cy="60" r="35" fill="#10b981" opacity="0.7"/>
  <circle cx="250" cy="60" r="35" fill="#ef4444" opacity="0.7"/>
  <text x="150" y="110" text-anchor="middle" font-size="9" fill="#64748b">
    Achica el previewer: los 3 círculos siempre caben ✓
  </text>
</svg>
```

---

## Pitfall 8: Coordenadas absolutas dentro de `<g transform>`

Al usar `transform="translate(x, y)"` en un `<g>`, los hijos usan coordenadas **relativas al nuevo origen**. Un error frecuente es seguir usando las coordenadas absolutas del padre dentro del grupo trasladado, duplicando efectivamente el desplazamiento.

### ❌ Incorrecto — coords absolutas dentro de translate
```svg
<svg width="200" height="150"
     style="border:2px solid #ef4444;background:#f8fafc;display:block;">
  <!-- Intención: colocar la cara centrada en (100, 75) -->
  <!-- Error: translate ya mueve a (100,75), y encima los elementos -->
  <!-- usan coords (100,75) → la cara queda en (200,150), fuera del SVG -->
  <g transform="translate(100, 75)">
    <!-- ❌ usando coords absolutas dentro del translate -->
    <circle cx="100" cy="75" r="40" fill="#ef4444" opacity="0.7"/>
    <text x="100" y="79" text-anchor="middle"
          font-size="10" fill="white">doble desplazamiento</text>
  </g>
  <text x="100" y="140" text-anchor="middle"
        font-size="9" fill="#64748b">La cara debería estar centrada</text>
</svg>
```

### ✅ Correcto — coords locales al origen del grupo
```svg
<svg width="200" height="150"
     style="border:2px solid #10b981;background:#f8fafc;display:block;">
  <!-- translate(100, 75) mueve el origen a (100,75) del SVG -->
  <!-- Los hijos usan coords LOCALES: (0,0) = el centro de la cara -->
  <g transform="translate(100, 75)">
    <!-- ✅ cx=0, cy=0 → el centro está en el origen local = (100,75) del SVG -->
    <circle cx="0" cy="0" r="40" fill="#10b981" opacity="0.7"/>
    <circle cx="-12" cy="-8" r="5" fill="white"/>
    <circle cx="12"  cy="-8" r="5" fill="white"/>
    <path d="M -14 10 Q 0 24 14 10"
          fill="none" stroke="white"
          stroke-width="3" stroke-linecap="round"/>
  </g>
  <text x="100" y="140" text-anchor="middle"
        font-size="9" fill="#64748b">Centrada correctamente ✓</text>
</svg>
```
