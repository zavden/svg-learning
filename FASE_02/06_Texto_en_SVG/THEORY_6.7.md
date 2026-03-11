# 6.7 Selección y búsqueda de texto

El texto SVG inline es **texto real**: seleccionable con el cursor, buscable con Ctrl+F, accesible para lectores de pantalla e indexable por motores de búsqueda.

---

## El texto SVG es texto real

```svg
<svg width="300" height="100"
     viewBox="0 0 300 100"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="10" y="10" width="280" height="75" rx="8" fill="#0f172a"/>
  <text x="150" y="40" text-anchor="middle" font-size="14" fill="white" font-weight="700">
    Texto seleccionable
  </text>
  <text x="150" y="58" text-anchor="middle" font-size="11" fill="#94a3b8">
    Puedes seleccionar y copiar este texto
  </text>
  <text x="150" y="75" text-anchor="middle" font-size="9" fill="#475569">
    (prueba con el cursor del ratón)
  </text>
</svg>
```

---

## Comportamiento por método de inclusión

```svg
<svg width="300" height="150"
     viewBox="0 0 300 150"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">SVG inline vs como imagen</text>

  <!-- Cabeceras -->
  <text x="10"  y="36" font-size="7.5" fill="#64748b">Capacidad</text>
  <text x="145" y="36" text-anchor="middle" font-size="7.5" fill="#3b82f6">Inline &lt;svg&gt;</text>
  <text x="255" y="36" text-anchor="middle" font-size="7.5" fill="#f59e0b">&lt;img src=".svg"&gt;</text>
  <line x1="0" y1="40" x2="300" y2="40" stroke="#e2e8f0" stroke-width="1"/>

  <!-- Filas -->
  <text x="10" y="55" font-size="7" fill="#64748b">Seleccionable</text>
  <text x="145" y="55" text-anchor="middle" font-size="8" fill="#10b981">✓</text>
  <text x="255" y="55" text-anchor="middle" font-size="8" fill="#ef4444">✗</text>

  <text x="10" y="70" font-size="7" fill="#64748b">Ctrl+F buscable</text>
  <text x="145" y="70" text-anchor="middle" font-size="8" fill="#10b981">✓</text>
  <text x="255" y="70" text-anchor="middle" font-size="8" fill="#ef4444">✗</text>

  <text x="10" y="85" font-size="7" fill="#64748b">Lector de pantalla</text>
  <text x="145" y="85" text-anchor="middle" font-size="8" fill="#10b981">✓</text>
  <text x="255" y="85" text-anchor="middle" font-size="7" fill="#f59e0b">parcial</text>

  <text x="10" y="100" font-size="7" fill="#64748b">Google lo indexa</text>
  <text x="145" y="100" text-anchor="middle" font-size="8" fill="#10b981">✓</text>
  <text x="255" y="100" text-anchor="middle" font-size="7" fill="#f59e0b">puede</text>

  <text x="10" y="115" font-size="7" fill="#64748b">Interactivo (JS/eventos)</text>
  <text x="145" y="115" text-anchor="middle" font-size="8" fill="#10b981">✓</text>
  <text x="255" y="115" text-anchor="middle" font-size="8" fill="#ef4444">✗</text>

  <line x1="200" y1="22" x2="200" y2="150" stroke="#e2e8f0" stroke-width="1"/>

  <text x="150" y="142" text-anchor="middle" font-size="7" fill="#94a3b8">
    Para texto accesible e interactivo: usar SVG inline
  </text>
</svg>
```

---

## Accesibilidad del texto SVG

El texto en `<text>` y `<tspan>` es leído directamente por los lectores de pantalla, en el orden en que aparece en el DOM.

```svg
<svg width="300" height="110"
     viewBox="0 0 300 110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Gráfico de datos accesible -->
  <title>Ventas mensuales — primer trimestre</title>

  <!-- Las barras (decorativas, no necesitan texto alternativo individual) -->
  <rect x="30" y="50" width="40" height="40" fill="#3b82f6"/>
  <rect x="110" y="30" width="40" height="60" fill="#3b82f6"/>
  <rect x="190" y="20" width="40" height="70" fill="#3b82f6"/>

  <!-- El texto SVG es parte del contenido accesible -->
  <text x="50"  y="100" text-anchor="middle" font-size="9" fill="#64748b">Ene</text>
  <text x="130" y="100" text-anchor="middle" font-size="9" fill="#64748b">Feb</text>
  <text x="210" y="100" text-anchor="middle" font-size="9" fill="#64748b">Mar</text>

  <!-- Valores como texto SVG (indexables y accesibles) -->
  <text x="50"  y="45" text-anchor="middle" font-size="9" fill="#1e293b">$40k</text>
  <text x="130" y="25" text-anchor="middle" font-size="9" fill="#1e293b">$60k</text>
  <text x="210" y="15" text-anchor="middle" font-size="9" fill="#1e293b">$70k</text>

  <text x="150" y="108" text-anchor="middle" font-size="7" fill="#94a3b8">
    Los valores de texto son leídos por lectores de pantalla
  </text>
</svg>
```

---

## Texto en SVG e indexación SEO

Los motores de búsqueda indexan el texto SVG inline exactamente igual que el texto HTML. Un mapa con nombres de ciudades o un gráfico con etiquetas de datos será indexado correctamente.

**Implicaciones prácticas:**
- Usa texto SVG real (no paths) para contenido que quieres que sea indexado
- Los títulos y descripciones en `<title>` y `<desc>` también son indexados
- Un SVG como `<img src="...">` solo es indexado si el motor de búsqueda procesa el archivo SVG (Google lo hace en algunos casos, pero con menos fiabilidad)
