# 3.6 Unidades en SVG

Las coordenadas y dimensiones en SVG pueden expresarse en distintas unidades. Elegir bien las unidades impacta directamente en la escalabilidad y responsividad del SVG.

**Las tres categorías principales:**

- **Sin unidad (user units):** son las más comunes dentro de las formas SVG. `<rect width="100">` usa 100 "unidades de usuario". Sin `viewBox`, 1 unidad = 1px CSS. Con `viewBox`, la unidad escala según la relación viewBox/viewport.
- **Unidades absolutas (`px`, `pt`, `mm`, `cm`, `in`, `pc`):** siempre tienen el mismo tamaño físico. Útiles para impresión; en pantalla, todo se referencia a 96dpi.
- **Unidades relativas (`%`, `em`):** dependen del contexto — el contenedor para `%`, el font-size para `em`.

---

## Ejemplo 1: Sin unidad vs `px` explícito — idénticos sin viewBox

Sin `viewBox`, los números sin unidad son equivalentes a píxeles CSS. `width="100"` y `width="100px"` producen exactamente el mismo resultado. La diferencia aparece cuando hay `viewBox` (ver Ejemplo 4).

```svg
<svg width="280" height="160"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block;">
  <!-- Sin viewBox: "100" y "100px" son idénticos (ambos = 100px CSS) -->

  <text x="140" y="18" text-anchor="middle"
        font-size="10" fill="#475569">Sin viewBox: sin unidad = px</text>

  <!-- Barra sin unidad: width="200" -->
  <rect x="10" y="30" width="200" height="28"
        fill="#3b82f6" opacity="0.8" rx="3"/>
  <text x="10" y="25" font-size="9" fill="#3b82f6">width="200"</text>
  <text x="215" y="49" font-size="9" fill="#64748b">= 200px</text>

  <!-- Barra con px explícito: width="200px" (mismo resultado) -->
  <rect x="10" y="70" width="200" height="28"
        fill="#10b981" opacity="0.8" rx="3"/>
  <text x="10" y="65" font-size="9" fill="#10b981">width="200px"</text>
  <text x="215" y="89" font-size="9" fill="#64748b">= 200px</text>

  <!-- Barra de referencia: 100 unidades -->
  <rect x="10" y="112" width="100" height="22"
        fill="#f59e0b" opacity="0.8" rx="3"/>
  <text x="10" y="108" font-size="9" fill="#f59e0b">width="100"</text>
  <text x="115" y="127" font-size="9" fill="#64748b">= 100px</text>

  <!-- Línea de referencia: 1px -->
  <rect x="10" y="145" width="1" height="10"
        fill="#475569"/>
  <text x="15" y="154" font-size="9" fill="#64748b">1px</text>
  <rect x="30" y="145" width="96" height="10"
        fill="#cbd5e1" rx="1"/>
  <text x="130" y="154" font-size="9" fill="#64748b">96px = 1in (96dpi)</text>
</svg>
```

---

## Ejemplo 2: Unidades absolutas de impresión — `mm`, `cm`, `pt`, `pc`

Las unidades de impresión tienen tamaño fijo en pantalla basado en 96dpi. En pantallas de alta densidad (Retina) el navegador compensa con el `devicePixelRatio`. Para SVG destinado a impresión, estas unidades garantizan tamaños físicos exactos.

```svg
<svg width="280" height="190"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block;">
  <!-- Equivalencias de unidades absolutas a píxeles CSS (96dpi) -->
  <!-- 1in = 96px | 1cm = ~37.8px | 1mm = ~3.78px -->
  <!-- 1pt = 1/72in ≈ 1.33px | 1pc = 12pt ≈ 16px -->

  <text x="140" y="16" text-anchor="middle"
        font-size="10" fill="#475569">Equivalencias a 96dpi (pantalla)</text>

  <!-- 1in = 96px -->
  <rect x="10" y="25" width="96" height="22"
        fill="#3b82f6" opacity="0.8" rx="2"/>
  <text x="110" y="40" font-size="9" fill="#3b82f6">
    1in = 96px
  </text>

  <!-- 2.54cm = 1in = 96px -->
  <rect x="10" y="55" width="96" height="22"
        fill="#10b981" opacity="0.8" rx="2"/>
  <text x="110" y="70" font-size="9" fill="#10b981">
    2.54cm ≈ 96px (1in)
  </text>

  <!-- 1cm ≈ 37.8px -->
  <rect x="10" y="85" width="38" height="22"
        fill="#f59e0b" opacity="0.8" rx="2"/>
  <text x="52" y="100" font-size="9" fill="#f59e0b">
    1cm ≈ 38px
  </text>

  <!-- 10mm = 1cm ≈ 37.8px -->
  <rect x="10" y="115" width="38" height="22"
        fill="#ef4444" opacity="0.8" rx="2"/>
  <text x="52" y="130" font-size="9" fill="#ef4444">
    10mm ≈ 38px
  </text>

  <!-- 1pc = 16px -->
  <rect x="10" y="145" width="16" height="22"
        fill="#a855f7" opacity="0.8" rx="2"/>
  <text x="30" y="160" font-size="9" fill="#a855f7">
    1pc = 16px
  </text>

  <!-- 12pt ≈ 16px -->
  <rect x="10" y="163" width="16" height="16"
        fill="#0ea5e9" opacity="0.8" rx="2"/>
  <text x="30" y="175" font-size="9" fill="#0ea5e9">
    12pt ≈ 16px (1pc)
  </text>

  <text x="140" y="188" text-anchor="middle"
        font-size="8" fill="#94a3b8">
    Para web: usar px o sin unidad. Para impresión: mm/cm/pt
  </text>
</svg>
```

---

## Ejemplo 3: Porcentajes — relativos al viewport del SVG

Los porcentajes en SVG se refieren al viewport del SVG, no al contenedor HTML. `width="50%"` en un `<rect>` equivale al 50% del ancho del `<svg>` que lo contiene. En el atributo `width`/`height` del propio `<svg>`, `%` se refiere al contenedor HTML.

```svg
<svg width="280" height="200"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block;">
  <!-- Los % dentro de formas SVG son relativos al viewport del SVG -->
  <!-- Este SVG mide 280×200 en pantalla (su viewport) -->
  <!-- 50% de 280 = 140px | 100% de 200 = 200px -->

  <!-- width="100%" ocupa el ancho completo del viewport (280px) -->
  <rect x="0" y="10" width="100%" height="22"
        fill="#3b82f6" opacity="0.7" rx="2"/>
  <text x="5" y="25" font-size="9" fill="white">
    width="100%" = 280px (ancho del SVG)
  </text>

  <!-- width="75%" = 75% de 280 = 210px -->
  <rect x="0" y="40" width="75%" height="22"
        fill="#10b981" opacity="0.7" rx="2"/>
  <text x="5" y="55" font-size="9" fill="white">
    width="75%" = 210px
  </text>

  <!-- width="50%" = 50% de 280 = 140px -->
  <rect x="0" y="70" width="50%" height="22"
        fill="#f59e0b" opacity="0.7" rx="2"/>
  <text x="5" y="85" font-size="9" fill="white">
    width="50%" = 140px
  </text>

  <!-- width="25%" = 25% de 280 = 70px -->
  <rect x="0" y="100" width="25%" height="22"
        fill="#ef4444" opacity="0.7" rx="2"/>
  <text x="5" y="115" font-size="9" fill="white">
    width="25%" = 70px
  </text>

  <!-- height="40%" = 40% de 200 = 80px -->
  <rect x="220" y="10" width="50" height="40%"
        fill="#a855f7" opacity="0.7" rx="2"/>
  <text x="245" y="90" text-anchor="middle"
        font-size="8" fill="#a855f7">h="40%"</text>
  <text x="245" y="100" text-anchor="middle"
        font-size="8" fill="#a855f7">=80px</text>

  <text x="140" y="145" text-anchor="middle"
        font-size="9" fill="#64748b">
    % en formas SVG → relativo al viewport del SVG
  </text>
  <text x="140" y="160" text-anchor="middle"
        font-size="9" fill="#94a3b8">
    % en el atributo width/height del svg → relativo al contenedor HTML
  </text>
</svg>
```

---

## Ejemplo 4: Unidades de usuario con `viewBox` — escalan automáticamente

Esta es la gran ventaja de las unidades de usuario: con `viewBox`, toda la geometría escala automáticamente cuando cambia el tamaño del viewport. Una unidad deja de ser 1px y pasa a ser lo que diga la relación viewBox/viewport.

```svg
<svg width="280" height="200"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block;">
  <!-- Demostración: mismo contenido con dos viewBox distintos -->
  <!-- Las MISMAS coordenadas producen tamaños visuales diferentes -->

  <text x="140" y="14" text-anchor="middle"
        font-size="10" fill="#475569">
    Mismo contenido, distinto viewBox → distinto tamaño visual
  </text>

  <!-- Caso A: SVG pequeño con viewBox="0 0 100 90" -->
  <!-- 1 unidad = 120px / 100 = 1.2px -->
  <text x="60" y="30" text-anchor="middle"
        font-size="9" fill="#3b82f6">viewBox 100×90</text>
  <svg x="10" y="35" width="120" height="90"
       viewBox="0 0 100 90"
       style="border:1px solid #3b82f6;background:#eff6ff;">
    <!-- Coords internas: 0-100 horizontal, 0-90 vertical -->
    <!-- El círculo en cx=50,cy=45,r=35 ocupa el espacio proporcional -->
    <circle cx="50" cy="45" r="35" fill="#3b82f6" opacity="0.8"/>
    <text x="50" y="49" text-anchor="middle"
          font-size="9" fill="white">r=35u</text>
  </svg>

  <!-- Caso B: SVG mismo tamaño pero viewBox="0 0 200 90" -->
  <!-- 1 unidad = 120px / 200 = 0.6px → el círculo se ve más pequeño -->
  <text x="210" y="30" text-anchor="middle"
        font-size="9" fill="#10b981">viewBox 200×90</text>
  <svg x="150" y="35" width="120" height="90"
       viewBox="0 0 200 90"
       style="border:1px solid #10b981;background:#f0fdf4;">
    <!-- Mismas coords (cx=50, cy=45, r=35) pero viewBox es el doble de ancho -->
    <!-- El círculo ocupa la mitad del ancho visual (50/200 = 25% del ancho) -->
    <circle cx="50" cy="45" r="35" fill="#10b981" opacity="0.8"/>
    <text x="50" y="49" text-anchor="middle"
          font-size="9" fill="white">r=35u</text>
  </svg>

  <text x="140" y="145" text-anchor="middle"
        font-size="9" fill="#64748b">
    Las mismas coordenadas (cx=50, r=35) → distintos tamaños visuales
  </text>
  <text x="140" y="160" text-anchor="middle"
        font-size="9" fill="#94a3b8">
    El viewBox define cuántas unidades caben → determina la escala
  </text>

  <!-- Fórmula de conversión -->
  <rect x="20" y="170" width="240" height="22"
        fill="#f1f5f9" rx="4"/>
  <text x="140" y="185" text-anchor="middle"
        font-size="9" fill="#334155">
    1 unidad = (tamaño viewport) / (tamaño viewBox)
  </text>
</svg>
```

---

## Ejemplo 5: Patrón recomendado — unidades de usuario + viewBox

El patrón más robusto para SVG en web: usar siempre unidades de usuario (sin sufijo) dentro de las formas, y controlar el tamaño del SVG con CSS desde fuera. El SVG escala correctamente a cualquier tamaño manteniendo proporciones.

```svg
<svg viewBox="0 0 200 120"
     style="width:100%;height:auto;border:2px solid #e2e8f0;
            background:#f8fafc;display:block;">
  <!-- Patrón recomendado para SVG en web: -->
  <!-- 1. viewBox define el espacio interno (aquí 200×120 unidades) -->
  <!-- 2. Sin width/height en el <svg>: el CSS controla el tamaño exterior -->
  <!-- 3. Todas las formas usan coordenadas en unidades de usuario -->
  <!-- 4. El navegador escala todo automáticamente, sin píxeles fijos -->

  <!-- Fondo del gráfico -->
  <rect width="200" height="120" fill="#f0f4f8" rx="4"/>

  <!-- Eje X del gráfico -->
  <line x1="15" y1="100" x2="190" y2="100"
        stroke="#94a3b8" stroke-width="1.5"/>

  <!-- Barras del gráfico — todas en unidades de usuario -->
  <rect x="20"  y="60" width="20" height="40" fill="#3b82f6" rx="2"/>
  <rect x="50"  y="40" width="20" height="60" fill="#3b82f6" rx="2"/>
  <rect x="80"  y="70" width="20" height="30" fill="#3b82f6" rx="2"/>
  <rect x="110" y="20" width="20" height="80" fill="#ef4444" rx="2"/>
  <rect x="140" y="50" width="20" height="50" fill="#3b82f6" rx="2"/>
  <rect x="170" y="30" width="20" height="70" fill="#ef4444" rx="2"/>

  <!-- Título en unidades de usuario (escala con el SVG) -->
  <text x="100" y="14" text-anchor="middle"
        font-size="8" fill="#475569">
    Patrón: viewBox + CSS width:100% → siempre responsivo
  </text>

  <!-- Nota en el gráfico -->
  <text x="100" y="115" text-anchor="middle"
        font-size="6" fill="#94a3b8">
    Cambia el ancho del previewer — el gráfico escala perfectamente
  </text>
</svg>
```
