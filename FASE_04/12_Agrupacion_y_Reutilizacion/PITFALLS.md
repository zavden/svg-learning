# Errores comunes — Agrupación y Reutilización

---

## 1. Intentar estilizar los hijos de `<use>` desde CSS externo

Los elementos dentro de una instancia `<use>` viven en un shadow DOM: los selectores CSS externos no pueden alcanzarlos. Apuntar a `use circle` o `use path` no tiene ningún efecto.

```svg
<svg width="300" height="180"
     viewBox="0 0 300 180"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <style>
    /* Esto NO alcanza al circle dentro del use */
    .mal-use circle { fill: red; }

    /* Esto SÍ: fill se hereda y currentColor se propaga */
    .bien-use { color: #ef4444; }
  </style>

  <defs>
    <symbol id="pin" viewBox="0 0 24 24">
      <path d="M 12 2 C 7 2 4 5 4 10 C 4 16 12 22 12 22 C 12 22 20 16 20 10 C 20 5 17 2 12 2 Z"
            fill="currentColor"/>
      <circle cx="12" cy="10" r="3" fill="white"/>
    </symbol>
  </defs>

  <!-- MAL -->
  <rect x="15" y="35" width="130" height="130" fill="#fef2f2" stroke="#ef4444" stroke-width="1.5" rx="4"/>
  <text x="80" y="52" text-anchor="middle" font-size="9" fill="#9d174d" font-weight="bold">MAL</text>
  <text x="80" y="66" text-anchor="middle" font-size="7" fill="#64748b">.mal-use circle{fill:red}</text>
  <use class="mal-use" href="#pin" x="50" y="80" width="60" height="60" style="color:#94a3b8"/>
  <text x="80" y="158" text-anchor="middle" font-size="7" fill="#ef4444">el circle interior ignoró CSS</text>

  <!-- BIEN -->
  <rect x="155" y="35" width="130" height="130" fill="#dcfce7" stroke="#10b981" stroke-width="1.5" rx="4"/>
  <text x="220" y="52" text-anchor="middle" font-size="9" fill="#166534" font-weight="bold">BIEN</text>
  <text x="220" y="66" text-anchor="middle" font-size="7" fill="#64748b">color + currentColor</text>
  <use class="bien-use" href="#pin" x="190" y="80" width="60" height="60"/>
  <text x="220" y="158" text-anchor="middle" font-size="7" fill="#166534">currentColor recoge el color</text>
</svg>
```

---

## 2. Esperar que `opacity` se herede elemento por elemento

`opacity` no es heredable como `fill` o `stroke`: se aplica al grupo renderizado como una sola capa. Esto produce un resultado visualmente distinto de aplicar opacity a cada hijo.

```svg
<svg width="300" height="180"
     viewBox="0 0 300 180"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Escenario: se quieren 3 círculos semitransparentes que SE MEZCLEN entre sí -->
  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">opacity NO funciona así en grupos</text>

  <!-- MAL: opacity en el grupo → se ve como una sola capa, sin mezclas internas -->
  <rect x="15" y="35" width="130" height="135" fill="#fef2f2" stroke="#ef4444" stroke-width="1.5" rx="4"/>
  <text x="80" y="52" text-anchor="middle" font-size="9" fill="#9d174d" font-weight="bold">MAL</text>
  <text x="80" y="66" text-anchor="middle" font-size="7" fill="#64748b">&lt;g opacity="0.5"&gt;</text>
  <g opacity="0.5">
    <circle cx="60"  cy="115" r="22" fill="#ef4444"/>
    <circle cx="80"  cy="115" r="22" fill="#3b82f6"/>
    <circle cx="100" cy="115" r="22" fill="#10b981"/>
  </g>
  <text x="80" y="158" text-anchor="middle" font-size="7" fill="#ef4444">los hijos no se mezclan entre sí</text>

  <!-- BIEN: opacity en cada hijo → las intersecciones muestran mezcla de color -->
  <rect x="155" y="35" width="130" height="135" fill="#dcfce7" stroke="#10b981" stroke-width="1.5" rx="4"/>
  <text x="220" y="52" text-anchor="middle" font-size="9" fill="#166534" font-weight="bold">BIEN</text>
  <text x="220" y="66" text-anchor="middle" font-size="7" fill="#64748b">opacity en cada hijo</text>
  <g>
    <circle cx="200" cy="115" r="22" fill="#ef4444" opacity="0.5"/>
    <circle cx="220" cy="115" r="22" fill="#3b82f6" opacity="0.5"/>
    <circle cx="240" cy="115" r="22" fill="#10b981" opacity="0.5"/>
  </g>
  <text x="220" y="158" text-anchor="middle" font-size="7" fill="#166534">mezclas visibles en intersección</text>
</svg>
```

---

## 3. Usar `<g>` para iconos cuando debería ser `<symbol>`

`<g>` no tiene `viewBox` propio: al instanciarlo con `<use width height>` no se escala como esperas. Para iconos reutilizables en distintos tamaños, `<symbol>` es la elección correcta.

```svg
<svg width="300" height="200"
     viewBox="0 0 300 200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <!-- MAL: grupo sin viewBox — width/height del use no hacen nada -->
    <g id="estrella-g">
      <polygon points="12,2 15,9 22,10 16,15 18,22 12,18 6,22 8,15 2,10 9,9"
               fill="#f59e0b" stroke="#92400e" stroke-width="1"/>
    </g>

    <!-- BIEN: símbolo con viewBox propio — el use escala correctamente -->
    <symbol id="estrella-sym" viewBox="0 0 24 24">
      <polygon points="12,2 15,9 22,10 16,15 18,22 12,18 6,22 8,15 2,10 9,9"
               fill="#f59e0b" stroke="#92400e" stroke-width="1"/>
    </symbol>
  </defs>

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">&lt;g&gt; vs &lt;symbol&gt; para iconos</text>

  <!-- MAL -->
  <rect x="15" y="35" width="130" height="155" fill="#fef2f2" stroke="#ef4444" stroke-width="1.5" rx="4"/>
  <text x="80" y="52" text-anchor="middle" font-size="9" fill="#9d174d" font-weight="bold">MAL: &lt;g&gt;</text>
  <text x="80" y="66" text-anchor="middle" font-size="7" fill="#64748b">width/height se ignoran</text>

  <use href="#estrella-g" x="35" y="75"  width="30" height="30"/>
  <use href="#estrella-g" x="75" y="75"  width="50" height="50"/>
  <use href="#estrella-g" x="35" y="125" width="80" height="80"/>
  <text x="80" y="180" text-anchor="middle" font-size="7" fill="#ef4444">todas iguales, tamaño 24×24</text>

  <!-- BIEN -->
  <rect x="155" y="35" width="130" height="155" fill="#dcfce7" stroke="#10b981" stroke-width="1.5" rx="4"/>
  <text x="220" y="52" text-anchor="middle" font-size="9" fill="#166534" font-weight="bold">BIEN: &lt;symbol&gt;</text>
  <text x="220" y="66" text-anchor="middle" font-size="7" fill="#64748b">width/height escalan el icono</text>

  <use href="#estrella-sym" x="165" y="75"  width="30" height="30"/>
  <use href="#estrella-sym" x="200" y="75"  width="45" height="45"/>
  <use href="#estrella-sym" x="175" y="125" width="70" height="70"/>
  <text x="220" y="180" text-anchor="middle" font-size="7" fill="#166534">se escalan como debe ser</text>
</svg>
```

---

## 4. Repetir estilos en cada elemento en vez de usar el padre

Definir `fill`, `stroke` y `font-family` en cada elemento satura el código y complica los cambios. Ponerlo una vez en el padre (o en `<svg>`) es mucho más mantenible.

```svg
<svg width="300" height="180"
     viewBox="0 0 300 180"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">no repitas, hereda</text>

  <!-- MAL: fill y stroke repetidos en cada elemento -->
  <rect x="15" y="35" width="130" height="135" fill="#fef2f2" stroke="#ef4444" stroke-width="1.5" rx="4"/>
  <text x="80" y="52" text-anchor="middle" font-size="9" fill="#9d174d" font-weight="bold">MAL</text>

  <circle cx="40"  cy="90" r="15" fill="#3b82f6" stroke="#1e40af" stroke-width="2"/>
  <rect   x="65"   y="75"  width="30" height="30" fill="#3b82f6" stroke="#1e40af" stroke-width="2"/>
  <polygon points="105,75 135,75 120,105" fill="#3b82f6" stroke="#1e40af" stroke-width="2"/>

  <text x="80" y="130" text-anchor="middle" font-size="6" fill="#9d174d">fill, stroke, stroke-width</text>
  <text x="80" y="140" text-anchor="middle" font-size="6" fill="#9d174d">en cada elemento</text>
  <text x="80" y="158" text-anchor="middle" font-size="7" fill="#ef4444">difícil de cambiar</text>

  <!-- BIEN: definidos una vez en el grupo -->
  <rect x="155" y="35" width="130" height="135" fill="#dcfce7" stroke="#10b981" stroke-width="1.5" rx="4"/>
  <text x="220" y="52" text-anchor="middle" font-size="9" fill="#166534" font-weight="bold">BIEN</text>

  <g fill="#3b82f6" stroke="#1e40af" stroke-width="2">
    <circle cx="180" cy="90" r="15"/>
    <rect   x="205"  y="75"  width="30" height="30"/>
    <polygon points="245,75 275,75 260,105"/>
  </g>

  <text x="220" y="130" text-anchor="middle" font-size="6" fill="#166534">&lt;g fill stroke stroke-width&gt;</text>
  <text x="220" y="140" text-anchor="middle" font-size="6" fill="#166534">…una sola vez</text>
  <text x="220" y="158" text-anchor="middle" font-size="7" fill="#166534">un solo punto de cambio</text>
</svg>
```

---

## 5. Olvidar que los atributos de presentación pierden frente a CSS

Un `fill="red"` como atributo es **menos prioritario** que una regla CSS. Si un SVG generado trae colores por atributo, una clase puede sobrescribirlos sin tocar el markup.

```svg
<svg width="300" height="180"
     viewBox="0 0 300 180"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <style>
    .tematizado { fill: #10b981; stroke: #166534; }
  </style>

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">CSS &gt; atributo de presentación</text>

  <!-- Sorpresa: el atributo fill se pierde -->
  <rect x="15" y="35" width="130" height="135" fill="#fef2f2" stroke="#ef4444" stroke-width="1.5" rx="4"/>
  <text x="80" y="52" text-anchor="middle" font-size="9" fill="#9d174d" font-weight="bold">SORPRESA</text>
  <text x="80" y="66" text-anchor="middle" font-size="7" fill="#64748b">fill="#3b82f6" class="tematizado"</text>
  <circle cx="80" cy="115" r="25" fill="#3b82f6" stroke="#1e40af" stroke-width="3" class="tematizado"/>
  <text x="80" y="158" text-anchor="middle" font-size="7" fill="#ef4444">esperabas azul — salió verde</text>

  <!-- Intencional: usar la cascada para tematizar sin tocar el SVG -->
  <rect x="155" y="35" width="130" height="135" fill="#dcfce7" stroke="#10b981" stroke-width="1.5" rx="4"/>
  <text x="220" y="52" text-anchor="middle" font-size="9" fill="#166534" font-weight="bold">VENTAJA</text>
  <text x="220" y="66" text-anchor="middle" font-size="7" fill="#64748b">el mismo SVG, tematizable</text>
  <circle cx="220" cy="115" r="25" fill="#3b82f6" stroke="#1e40af" stroke-width="3" class="tematizado"/>
  <text x="220" y="158" text-anchor="middle" font-size="7" fill="#166534">CSS reescribe el color base</text>
</svg>
```
