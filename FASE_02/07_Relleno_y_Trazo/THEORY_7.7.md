# 7.7 `paint-order`

Por defecto SVG pinta el fill **antes** que el stroke. `paint-order` invierte ese orden para que el stroke quede debajo del fill — especialmente útil en texto con contorno.

---

## El problema por defecto

El stroke se centra en el borde: la mitad interior cubre el borde del fill. En texto, esto puede hacer que el trazo tape partes de los caracteres.

```svg
<svg width="300" height="120"
     viewBox="0 0 300 120"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#0f172a;display:block; font-family:sans-serif;">

  <!-- Sin paint-order: stroke encima del fill (default) -->
  <!-- Stroke interior cubre el borde del fill -->
  <text x="150" y="52" text-anchor="middle" font-size="38" font-weight="900"
        fill="#3b82f6" stroke="white" stroke-width="6">
    SVG
  </text>
  <text x="150" y="68" text-anchor="middle" font-size="8" fill="#64748b">
    sin paint-order — stroke cubre el fill
  </text>

  <!-- Con paint-order="stroke fill": stroke dibujado primero, fill encima -->
  <!-- El stroke queda completamente exterior al fill -->
  <text x="150" y="102" text-anchor="middle" font-size="38" font-weight="900"
        fill="#3b82f6" stroke="white" stroke-width="6"
        paint-order="stroke fill">
    SVG
  </text>
  <text x="150" y="118" text-anchor="middle" font-size="8" fill="#10b981">
    paint-order="stroke fill" — stroke exterior ✓
  </text>
</svg>
```

---

## Cómo funciona `paint-order`

El valor indica el **orden de pintado** de menor a mayor (el último queda encima).

```svg
<svg width="300" height="110"
     viewBox="0 0 300 110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">paint-order</text>

  <!-- Orden por defecto: fill stroke markers -->
  <rect x="20" y="30" width="80" height="55" fill="#3b82f6" stroke="#ef4444" stroke-width="12"/>
  <text x="60" y="97" text-anchor="middle" font-size="7" fill="#64748b">defecto</text>
  <text x="60" y="107" text-anchor="middle" font-size="6.5" fill="#94a3b8">fill → stroke</text>
  <!-- El stroke rojo cubre el borde azul -->

  <!-- paint-order="stroke fill": stroke primero, fill encima -->
  <rect x="120" y="30" width="80" height="55" fill="#3b82f6" stroke="#ef4444" stroke-width="12"
        paint-order="stroke fill"/>
  <text x="160" y="97" text-anchor="middle" font-size="7" fill="#64748b">stroke fill</text>
  <text x="160" y="107" text-anchor="middle" font-size="6.5" fill="#94a3b8">stroke → fill</text>
  <!-- El stroke queda debajo, solo la mitad exterior visible -->

  <!-- Anotaciones de diferencia -->
  <text x="60" y="44" text-anchor="middle" font-size="7" fill="white">stroke</text>
  <text x="60" y="54" text-anchor="middle" font-size="7" fill="white">encima</text>
  <text x="160" y="44" text-anchor="middle" font-size="7" fill="white">fill</text>
  <text x="160" y="54" text-anchor="middle" font-size="7" fill="white">encima</text>

  <!-- Comparativa de bordes -->
  <text x="240" y="44" text-anchor="middle" font-size="7.5" fill="#64748b">stroke rojo:</text>
  <text x="240" y="55" text-anchor="middle" font-size="7" fill="#64748b">izq: mitad dentro</text>
  <text x="240" y="66" text-anchor="middle" font-size="7" fill="#64748b">+mitad fuera</text>
  <text x="240" y="78" text-anchor="middle" font-size="7" fill="#64748b">der: solo mitad</text>
  <text x="240" y="89" text-anchor="middle" font-size="7" fill="#64748b">fuera visible</text>
</svg>
```

---

## Uso principal: texto con contorno limpio

```svg
<svg width="300" height="130"
     viewBox="0 0 300 130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Sin paint-order: stroke tapa partes del carácter -->
  <text x="150" y="45" text-anchor="middle" font-size="32" font-weight="900"
        fill="white" stroke="#1e293b" stroke-width="4">
    Hello
  </text>
  <text x="150" y="60" text-anchor="middle" font-size="7.5" fill="#ef4444">
    stroke encima → detalles del texto tapados
  </text>

  <!-- Con paint-order: stroke exterior, fill completo visible -->
  <text x="150" y="100" text-anchor="middle" font-size="32" font-weight="900"
        fill="white" stroke="#1e293b" stroke-width="4"
        paint-order="stroke fill">
    Hello
  </text>
  <text x="150" y="115" text-anchor="middle" font-size="7.5" fill="#10b981">
    paint-order="stroke fill" → texto completamente limpio ✓
  </text>
  <text x="150" y="127" text-anchor="middle" font-size="7" fill="#94a3b8">
    el stroke actúa como "sombra" o "contorno" exterior
  </text>
</svg>
```
