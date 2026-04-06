# Errores comunes — SVG responsivo y accesibilidad

---

## 1. Dejar `width` y `height` fijos en el elemento `<svg>`

El error más frecuente: copiar un SVG de una herramienta de diseño que exporta con `width="400" height="300"` y pretender que se comporte como responsivo. El SVG ocupará siempre **exactamente** 400×300 píxeles, no importa qué diga el CSS del contenedor (a menos que el CSS sea más específico).

```svg
<svg width="300" height="170"
     viewBox="0 0 300 170"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">quita las dimensiones fijas</text>

  <!-- Mal -->
  <rect x="15" y="32" width="270" height="58" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="48" font-size="9" font-weight="bold" fill="#991b1b">✗ Mal</text>
  <text x="25" y="62" font-size="7" font-family="monospace" fill="#475569">&lt;svg width="400" height="300" viewBox="0 0 400 300"&gt;</text>
  <text x="25" y="75" font-size="7" fill="#475569">→ el SVG ignora el CSS del contenedor y ocupa 400×300</text>

  <!-- Bien -->
  <rect x="15" y="98" width="270" height="60" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="114" font-size="9" font-weight="bold" fill="#166534">✓ Bien</text>
  <text x="25" y="128" font-size="7" font-family="monospace" fill="#475569">&lt;svg viewBox="0 0 400 300"&gt;  /* sin width/height */</text>
  <text x="25" y="141" font-size="7" font-family="monospace" fill="#475569">svg { width: 100%; height: auto; }</text>
  <text x="25" y="154" font-size="7" fill="#166534">→ el SVG se adapta al contenedor</text>
</svg>
```

**Variación**: dejar `width` pero quitar `height`. Si solo sobra uno, el SVG mantiene su ancho fijo pero calcula la altura — a veces funciona, a veces no. Limpia ambos y controla desde CSS.

---

## 2. Omitir el `viewBox`

Sin `viewBox`, el navegador no tiene forma de saber la proporción interna del SVG. Los elementos se colocan en sus coordenadas literales y no escalan cuando el contenedor cambia de tamaño.

```svg
<svg width="300" height="150"
     viewBox="0 0 300 150"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">viewBox es obligatorio para responsividad</text>

  <rect x="15" y="32" width="270" height="48" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="48" font-size="9" font-weight="bold" fill="#991b1b">✗ Sin viewBox</text>
  <text x="25" y="62" font-size="7" font-family="monospace" fill="#475569">&lt;svg width="100%" height="auto"&gt;</text>
  <text x="25" y="75" font-size="7" fill="#475569">el SVG no sabe cómo escalar su contenido interno</text>

  <rect x="15" y="88" width="270" height="50" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="104" font-size="9" font-weight="bold" fill="#166534">✓ Con viewBox</text>
  <text x="25" y="118" font-size="7" font-family="monospace" fill="#475569">&lt;svg viewBox="0 0 400 300"&gt;</text>
  <text x="25" y="131" font-size="7" fill="#475569">define la caja lógica interna; height: auto puede calcular la altura</text>
</svg>
```

Regla mental: **viewBox siempre, width/height casi nunca** (cuando el tamaño lo decide el CSS).

---

## 3. Olvidar `display: block` y ver espacio fantasma

Los elementos SVG inline reservan espacio vertical para descendentes tipográficos (j, g, p, y…), igual que una letra. Esto produce un pequeño espacio blanco inesperado debajo del SVG dentro de un contenedor.

```svg
<svg width="300" height="170"
     viewBox="0 0 300 170"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">el espacio fantasma inline</text>

  <!-- Inline (mal) -->
  <rect x="15" y="32" width="130" height="120" fill="white" stroke="#ef4444" stroke-width="1.5"/>
  <text x="80" y="46" text-anchor="middle" font-size="7" fill="#991b1b">sin display: block</text>
  <rect x="30" y="56" width="100" height="50" fill="#dbeafe"/>
  <line x1="30" y1="110" x2="130" y2="110" stroke="#ef4444" stroke-dasharray="2,2"/>
  <text x="80" y="125" text-anchor="middle" font-size="6" fill="#ef4444">← espacio extra</text>
  <text x="80" y="144" text-anchor="middle" font-size="6" fill="#64748b">borde del contenedor</text>

  <!-- Block (bien) -->
  <rect x="155" y="32" width="130" height="120" fill="white" stroke="#10b981" stroke-width="1.5"/>
  <text x="220" y="46" text-anchor="middle" font-size="7" fill="#166534">display: block</text>
  <rect x="170" y="56" width="100" height="92" fill="#dcfce7"/>
  <text x="220" y="142" text-anchor="middle" font-size="6" fill="#166534">sin espacio</text>
</svg>
```

La solución es una línea de CSS: `svg { display: block; }` (o convertirlo en cualquier otro tipo de bloque como `flex` o `grid`).

---

## 4. Usar colores fijos en SVGs que necesitan adaptarse

Un ícono con `fill="#3b82f6"` siempre será ese azul exacto, ignorando modo oscuro, alto contraste, color del botón contenedor, etc. Si el SVG debe adaptarse a contextos variados, los colores fijos son deuda técnica.

```svg
<svg width="300" height="180"
     viewBox="0 0 300 180"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">color fijo vs. currentColor</text>

  <rect x="15" y="32" width="270" height="64" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="48" font-size="9" font-weight="bold" fill="#991b1b">✗ Color fijo</text>
  <text x="25" y="62" font-size="7" font-family="monospace" fill="#475569">&lt;svg&gt;&lt;circle fill="#3b82f6"/&gt;&lt;/svg&gt;</text>
  <text x="25" y="78" font-size="7" fill="#475569">mismo azul en todos los contextos; no hereda nada</text>
  <text x="25" y="90" font-size="7" fill="#991b1b">ignora modo oscuro, alto contraste y tematización</text>

  <rect x="15" y="104" width="270" height="66" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="120" font-size="9" font-weight="bold" fill="#166534">✓ currentColor</text>
  <text x="25" y="134" font-size="7" font-family="monospace" fill="#475569">&lt;svg&gt;&lt;circle fill="currentColor"/&gt;&lt;/svg&gt;</text>
  <text x="25" y="150" font-size="7" fill="#475569">el padre controla el color: color: red → ícono rojo</text>
  <text x="25" y="162" font-size="7" fill="#166534">se adapta a botones, alertas, temas y alto contraste</text>
</svg>
```

---

## 5. Añadir `role="img"` y luego olvidar el `<title>`

`role="img"` indica al lector de pantalla que el SVG es una imagen con significado, pero sin un `<title>` (o `aria-label`) la imagen queda **sin nombre accesible** — el lector puede anunciar "imagen" sin más contenido. Peor: con `aria-labelledby` apuntando a un id inexistente, algunos lectores anuncian "en blanco".

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">role="img" sin nombre</text>

  <!-- Mal -->
  <rect x="15" y="32" width="270" height="64" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="48" font-size="9" font-weight="bold" fill="#991b1b">✗ role sin título</text>
  <text x="25" y="62" font-size="7" font-family="monospace" fill="#475569">&lt;svg role="img"&gt;&lt;circle/&gt;&lt;/svg&gt;</text>
  <text x="25" y="76" font-size="7" fill="#475569">el lector anuncia "imagen" o se queda en blanco</text>
  <text x="25" y="88" font-size="7" fill="#991b1b">el usuario no sabe qué representa</text>

  <!-- Bien -->
  <rect x="15" y="104" width="270" height="100" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="120" font-size="9" font-weight="bold" fill="#166534">✓ role + título</text>
  <text x="25" y="134" font-size="7" font-family="monospace" fill="#475569">&lt;svg role="img" aria-labelledby="t"&gt;</text>
  <text x="25" y="146" font-size="7" font-family="monospace" fill="#475569">  &lt;title id="t"&gt;Logo de Acme&lt;/title&gt;</text>
  <text x="25" y="158" font-size="7" font-family="monospace" fill="#475569">  &lt;circle/&gt;</text>
  <text x="25" y="170" font-size="7" font-family="monospace" fill="#475569">&lt;/svg&gt;</text>
  <text x="25" y="188" font-size="7" fill="#166534">el lector dice: "Logo de Acme, imagen"</text>
</svg>
```

**Regla**: si pones `role="img"`, también pon un nombre accesible (`<title>` + `aria-labelledby`, o `aria-label`). Si el SVG es decorativo, mejor `aria-hidden="true"` que `role="img"` sin título.
