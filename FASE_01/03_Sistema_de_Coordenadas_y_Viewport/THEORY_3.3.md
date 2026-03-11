# 3.3 El atributo `preserveAspectRatio`

Cuando las proporciones del `viewBox` y del `viewport` son distintas, el navegador necesita decidir qué hacer con el espacio sobrante o faltante. `preserveAspectRatio` da esas instrucciones.

**Sintaxis:** `preserveAspectRatio="<align> <meetOrSlice>"`

- `<align>`: combina alineación horizontal (`xMin`, `xMid`, `xMax`) con vertical (`YMin`, `YMid`, `YMax`). El valor especial `none` desactiva toda preservación.
- `<meetOrSlice>`: `meet` (cabe todo, puede haber espacio vacío) o `slice` (cubre todo, puede recortarse)
- **Valor por defecto:** `xMidYMid meet`

---

## Ejemplo 1: xMidYMid meet — centrado con pillarboxing (valor por defecto)

El viewport es 260×130 (apaisado) y el viewBox es 100×100 (cuadrado). Con `meet`, el contenido cabe completo: escala hasta 130×130 (escala=1.3). El espacio restante de 65px a cada lado queda vacío — el fondo gris del SVG lo hace visible (**pillarboxing**).

```svg
<svg width="260" height="130" viewBox="0 0 100 100"
     preserveAspectRatio="xMidYMid meet"
     style="border:2px solid #e2e8f0;background:#94a3b8;display:block;">
  <!-- viewport: 260×130 | viewBox: 100×100 (cuadrado) -->
  <!-- meet: todo cabe → scale=min(2.6,1.3)=1.3 → activo: 130×130px -->
  <!-- El fondo gris #94a3b8 son las "franjas" vacías (pillarboxing) -->
  <!-- xMidYMid: el contenido se centra → 65px de gris a cada lado -->

  <!-- Área activa del viewBox (visible como zona blanco-azulada) -->
  <rect width="100" height="100" fill="#eff6ff"/>

  <!-- Contenido: cara simple para que el recorte sea obvio -->
  <circle cx="50" cy="48" r="40" fill="#3b82f6" opacity="0.9"/>
  <circle cx="37" cy="41" r="6"  fill="white"/>
  <circle cx="63" cy="41" r="6"  fill="white"/>
  <path d="M 30 60 Q 50 78 70 60"
        fill="none" stroke="white"
        stroke-width="5" stroke-linecap="round"/>
  <text x="50" y="94" text-anchor="middle"
        font-size="6" fill="#1e40af">xMidYMid meet (default)</text>
</svg>
```

---

## Ejemplo 2: xMinYMid meet — anclado al borde izquierdo

Mismas proporciones que el ejemplo anterior pero con `xMinYMid`. El contenido se ancla al borde izquierdo del viewport. La franja vacía aparece solo a la derecha.

```svg
<svg width="260" height="130" viewBox="0 0 100 100"
     preserveAspectRatio="xMinYMid meet"
     style="border:2px solid #10b981;background:#94a3b8;display:block;">
  <!-- viewport: 260×130 | viewBox: 100×100 -->
  <!-- meet: scale=1.3 → contenido activo: 130×130px -->
  <!-- xMinYMid: contenido pegado al BORDE IZQUIERDO -->
  <!-- La franja gris aparece solo a la DERECHA -->

  <rect width="100" height="100" fill="#f0fdf4"/>

  <circle cx="50" cy="48" r="40" fill="#10b981" opacity="0.9"/>
  <circle cx="37" cy="41" r="6"  fill="white"/>
  <circle cx="63" cy="41" r="6"  fill="white"/>
  <path d="M 30 60 Q 50 78 70 60"
        fill="none" stroke="white"
        stroke-width="5" stroke-linecap="round"/>
  <text x="50" y="94" text-anchor="middle"
        font-size="6" fill="#065f46">xMinYMid meet → izquierda</text>
</svg>
```

---

## Ejemplo 3: xMaxYMid meet — anclado al borde derecho

El contenido se ancla al borde derecho del viewport. La franja vacía aparece solo a la izquierda.

```svg
<svg width="260" height="130" viewBox="0 0 100 100"
     preserveAspectRatio="xMaxYMid meet"
     style="border:2px solid #f59e0b;background:#94a3b8;display:block;">
  <!-- viewport: 260×130 | viewBox: 100×100 -->
  <!-- meet: scale=1.3 → contenido activo: 130×130px -->
  <!-- xMaxYMid: contenido pegado al BORDE DERECHO -->
  <!-- La franja gris aparece solo a la IZQUIERDA -->

  <rect width="100" height="100" fill="#fffbeb"/>

  <circle cx="50" cy="48" r="40" fill="#f59e0b" opacity="0.9"/>
  <circle cx="37" cy="41" r="6"  fill="white"/>
  <circle cx="63" cy="41" r="6"  fill="white"/>
  <path d="M 30 60 Q 50 78 70 60"
        fill="none" stroke="white"
        stroke-width="5" stroke-linecap="round"/>
  <text x="50" y="94" text-anchor="middle"
        font-size="6" fill="#92400e">xMaxYMid meet → derecha</text>
</svg>
```

---

## Ejemplo 4: xMidYMid slice — cubre todo el viewport, recorta el exceso

Con `slice`, el contenido escala para **cubrir** todo el viewport. La escala es `max(2.6, 1.3) = 2.6`, por lo que el contenido renderiza a 260×260px, pero el viewport solo tiene 130px de alto. Las líneas amarillas indican dónde el viewport corta el contenido.

```svg
<svg width="260" height="130" viewBox="0 0 100 100"
     preserveAspectRatio="xMidYMid slice"
     style="border:2px solid #ef4444;background:#fef2f2;display:block;">
  <!-- viewport: 260×130 | viewBox: 100×100 -->
  <!-- slice: cubre todo → scale=max(2.6,1.3)=2.6 → 260×260px -->
  <!-- Solo se ve la franja central y=25..75 del viewBox (130px) -->
  <!-- El círculo (r=40) se recorta: solo y=25..75 es visible -->

  <rect width="100" height="100" fill="#fee2e2"/>

  <circle cx="50" cy="48" r="40" fill="#ef4444" opacity="0.9"/>
  <circle cx="37" cy="41" r="6"  fill="white"/>
  <circle cx="63" cy="41" r="6"  fill="white"/>
  <!-- La sonrisa (y=60..78) queda parcialmente fuera (y>75 cortado) -->
  <path d="M 30 60 Q 50 78 70 60"
        fill="none" stroke="white"
        stroke-width="5" stroke-linecap="round"/>

  <!-- Líneas que marcan dónde corta el viewport (y=25 y y=75) -->
  <line x1="0" y1="25" x2="100" y2="25"
        stroke="#fbbf24" stroke-width="1.5" stroke-dasharray="3,2"/>
  <line x1="0" y1="75" x2="100" y2="75"
        stroke="#fbbf24" stroke-width="1.5" stroke-dasharray="3,2"/>
  <text x="52" y="29" font-size="5" fill="#fbbf24">corte → y=25</text>
  <text x="52" y="72" font-size="5" fill="#fbbf24">corte → y=75</text>
</svg>
```

---

## Ejemplo 5: none — distorsión total para llenar el viewport

`none` desactiva toda preservación de aspecto. El viewBox se estira de forma independiente en X e Y para llenar exactamente el viewport. El contenido se distorsiona: en este caso scaleX=2.6, scaleY=1.3, por lo que el círculo se convierte en una elipse ancha.

```svg
<svg width="260" height="130" viewBox="0 0 100 100"
     preserveAspectRatio="none"
     style="border:2px solid #a855f7;background:#faf5ff;display:block;">
  <!-- preserveAspectRatio="none" → se ignoran las proporciones -->
  <!-- El viewBox 100×100 se ESTIRA para llenar 260×130 exactamente -->
  <!-- scaleX = 260/100 = 2.6 | scaleY = 130/100 = 1.3 → distorsión -->
  <!-- El círculo se convierte en una elipse (ratio 2:1) -->

  <rect width="100" height="100" fill="#f3e8ff"/>

  <!-- Este círculo se verá como elipse por la distorsión horizontal -->
  <circle cx="50" cy="48" r="40" fill="#a855f7" opacity="0.9"/>
  <circle cx="37" cy="41" r="6"  fill="white"/>
  <circle cx="63" cy="41" r="6"  fill="white"/>
  <path d="M 30 60 Q 50 78 70 60"
        fill="none" stroke="white"
        stroke-width="5" stroke-linecap="round"/>
  <text x="50" y="94" text-anchor="middle"
        font-size="6" fill="#6b21a8">none → distorsión (no recomendado)</text>
</svg>
```

---

## Ejemplo 6: Viewport portrait con viewBox cuadrado — letterboxing

Con un viewport vertical (130×260) y un viewBox cuadrado (100×100) con `meet`, ahora las franjas vacías aparecen **arriba y abajo** en lugar de a los lados (**letterboxing** en vez de pillarboxing).

```svg
<svg width="130" height="260" viewBox="0 0 100 100"
     preserveAspectRatio="xMidYMid meet"
     style="border:2px solid #e2e8f0;background:#94a3b8;display:block;">
  <!-- viewport: 130×260 (portrait) | viewBox: 100×100 (cuadrado) -->
  <!-- meet: scale=min(1.3,2.6)=1.3 → activo: 130×130px -->
  <!-- xMidYMid: centrado → 65px de gris ARRIBA Y ABAJO (letterboxing) -->
  <!-- Las franjas ahora son horizontales, no verticales -->

  <rect width="100" height="100" fill="#eff6ff"/>

  <circle cx="50" cy="48" r="40" fill="#3b82f6" opacity="0.9"/>
  <circle cx="37" cy="41" r="6"  fill="white"/>
  <circle cx="63" cy="41" r="6"  fill="white"/>
  <path d="M 30 60 Q 50 78 70 60"
        fill="none" stroke="white"
        stroke-width="5" stroke-linecap="round"/>
  <text x="50" y="94" text-anchor="middle"
        font-size="6" fill="#1e40af">letterboxing (franjas arriba/abajo)</text>
</svg>
```

---

## Ejemplo 7: Caso práctico — meet para logos, slice para fondos

`meet` garantiza que el contenido completo sea siempre visible (ideal para logos e iconos). `slice` garantiza que no haya espacio vacío (ideal para fondos e imágenes decorativas).

```svg
<svg width="280" height="170"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block;">
  <!-- Comparación práctica de meet vs slice en casos reales -->

  <!-- === Logo en botón: usar meet === -->
  <text x="70" y="16" text-anchor="middle"
        font-size="11" fill="#475569">Logo → meet ✓</text>
  <!-- Contenedor del botón -->
  <rect x="10" y="22" width="120" height="70"
        fill="#eff6ff" stroke="#3b82f6" stroke-width="2" rx="4"/>
  <!-- Franjas de pillarboxing (meet deja espacio) -->
  <rect x="10" y="22" width="25" height="70"
        fill="#dbeafe" opacity="0.5" rx="4"/>
  <rect x="105" y="22" width="25" height="70"
        fill="#dbeafe" opacity="0.5"/>
  <!-- Logo centrado (no distorsionado) -->
  <circle cx="70" cy="57" r="28" fill="#3b82f6" opacity="0.9"/>
  <circle cx="61" cy="52" r="4"  fill="white"/>
  <circle cx="79" cy="52" r="4"  fill="white"/>
  <path d="M 58 64 Q 70 74 82 64"
        fill="none" stroke="white"
        stroke-width="3" stroke-linecap="round"/>
  <text x="70" y="105" text-anchor="middle"
        font-size="9" fill="#3b82f6">Siempre visible completo</text>

  <!-- Separador -->
  <line x1="140" y1="18" x2="140" y2="130"
        stroke="#e2e8f0" stroke-width="1"/>

  <!-- === Fondo hero: usar slice === -->
  <text x="210" y="16" text-anchor="middle"
        font-size="11" fill="#475569">Fondo → slice ✓</text>
  <!-- Contenedor del área hero -->
  <rect x="150" y="22" width="120" height="70"
        fill="#1e293b" stroke="#0f172a" stroke-width="2" rx="4"/>
  <!-- Fondo que cubre todo el área (algún borde se corta) -->
  <circle cx="210" cy="57" r="52" fill="#3b82f6" opacity="0.5"/>
  <circle cx="210" cy="57" r="35" fill="#1d4ed8" opacity="0.6"/>
  <text x="210" y="61" text-anchor="middle"
        font-size="9" fill="white">Llena todo el espacio</text>
  <text x="210" y="105" text-anchor="middle"
        font-size="9" fill="#ef4444">Sin franjas vacías</text>

  <!-- Conclusión -->
  <text x="140" y="140" text-anchor="middle"
        font-size="10" fill="#64748b">
    meet = contenido completo | slice = sin espacios vacíos
  </text>
  <text x="140" y="158" text-anchor="middle"
        font-size="9" fill="#94a3b8">
    El valor por defecto (xMidYMid meet) funciona para la mayoría de casos
  </text>
</svg>
```
