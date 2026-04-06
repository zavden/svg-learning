# 12.1 El elemento `<g>` (grupo)

El `<g>` es el contenedor lógico de SVG: agrupa elementos como una unidad, propaga atributos heredables a sus hijos y permite aplicar transformaciones o efectos a todo el conjunto a la vez.

---

## `<g>` como contenedor sin forma propia

```svg
<svg width="300" height="130"
     viewBox="0 0 300 130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">&lt;g&gt; agrupa, no dibuja</text>

  <!-- Un grupo es una caja lógica: no tiene borde, fondo ni geometría propia -->
  <g id="cara">
    <circle cx="70" cy="75" r="30" fill="#fef9c3" stroke="#f59e0b" stroke-width="2"/>
    <circle cx="60" cy="70" r="3" fill="#1e293b"/>
    <circle cx="80" cy="70" r="3" fill="#1e293b"/>
    <path d="M 58 85 Q 70 95 82 85" fill="none" stroke="#1e293b" stroke-width="2" stroke-linecap="round"/>
  </g>
  <text x="70" y="120" text-anchor="middle" font-size="9" fill="#64748b">&lt;g id="cara"&gt;</text>

  <!-- Cada elemento suelto seguiría dibujando igual, el <g> solo los agrupa -->
  <g transform="translate(160, 0)">
    <rect x="10" y="45" width="110" height="60" fill="none" stroke="#94a3b8" stroke-dasharray="3,3"/>
    <text x="65" y="40" text-anchor="middle" font-size="8" fill="#64748b">el grupo no es visible</text>
    <circle cx="35" cy="75" r="12" fill="#dbeafe" stroke="#3b82f6" stroke-width="1.5"/>
    <circle cx="65" cy="75" r="12" fill="#dcfce7" stroke="#10b981" stroke-width="1.5"/>
    <circle cx="95" cy="75" r="12" fill="#fce7f3" stroke="#ec4899" stroke-width="1.5"/>
    <text x="65" y="120" text-anchor="middle" font-size="9" fill="#64748b">solo contenedor lógico</text>
  </g>
</svg>
```

---

## Herencia: atributos de presentación en el grupo

```svg
<svg width="300" height="150"
     viewBox="0 0 300 150"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">los hijos heredan del &lt;g&gt;</text>

  <!-- El grupo define fill, stroke y stroke-width: todos los hijos lo reciben -->
  <g fill="#dbeafe" stroke="#1e40af" stroke-width="2">
    <circle cx="40" cy="70" r="22"/>
    <rect x="75" y="48" width="45" height="45"/>
    <polygon points="155,50 180,93 130,93"/>
  </g>
  <text x="100" y="115" text-anchor="middle" font-size="9" fill="#64748b">fill y stroke heredados</text>

  <!-- Un hijo puede sobreescribir cualquier atributo heredado -->
  <g fill="#fce7f3" stroke="#9d174d" stroke-width="2">
    <circle cx="215" cy="70" r="22"/>
    <!-- este rect rompe la herencia: fill propio -->
    <rect x="250" y="48" width="40" height="45" fill="#fef9c3" stroke="#f59e0b"/>
  </g>
  <text x="245" y="115" text-anchor="middle" font-size="9" fill="#64748b">hijo sobrescribe fill</text>

  <text x="150" y="140" text-anchor="middle" font-size="8" fill="#94a3b8">
    el atributo propio del hijo siempre gana sobre el heredado
  </text>
</svg>
```

---

## `transform` en un grupo: afecta a todos los hijos

```svg
<svg width="300" height="170"
     viewBox="0 0 300 170"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">transform en grupo = mover todo</text>

  <!-- Grupo original sin transformar -->
  <g>
    <rect x="15" y="40" width="60" height="60" fill="#dbeafe" stroke="#3b82f6" stroke-width="1.5"/>
    <circle cx="45" cy="70" r="15" fill="#3b82f6" fill-opacity="0.5"/>
    <text x="45" y="115" text-anchor="middle" font-size="8" fill="#3b82f6">original</text>
  </g>

  <!-- El mismo grupo con translate y rotate: todos los hijos se mueven juntos -->
  <g transform="translate(110, 10) rotate(12, 45, 70)">
    <rect x="15" y="40" width="60" height="60" fill="#dcfce7" stroke="#10b981" stroke-width="1.5"/>
    <circle cx="45" cy="70" r="15" fill="#10b981" fill-opacity="0.5"/>
    <text x="45" y="115" text-anchor="middle" font-size="8" fill="#166534">translate + rotate</text>
  </g>

  <!-- Un grupo escalado: los hijos conservan su relación, solo se agrandan -->
  <g transform="translate(215, 45) scale(0.7)">
    <rect x="0" y="0" width="60" height="60" fill="#fef9c3" stroke="#f59e0b" stroke-width="2"/>
    <circle cx="30" cy="30" r="15" fill="#f59e0b" fill-opacity="0.5"/>
  </g>
  <text x="235" y="135" text-anchor="middle" font-size="8" fill="#92400e">scale(0.7)</text>

  <text x="150" y="155" text-anchor="middle" font-size="8" fill="#94a3b8">
    las coordenadas internas del grupo no cambian: se transforma el sistema
  </text>
</svg>
```

---

## `opacity` y efectos en grupo vs en hijos

```svg
<svg width="300" height="170"
     viewBox="0 0 300 170"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">opacity en grupo ≠ en hijos</text>

  <!-- Caso 1: opacity en cada círculo por separado → se ven mezclas donde se solapan -->
  <g>
    <text x="70" y="40" text-anchor="middle" font-size="9" fill="#64748b">opacity en cada hijo</text>
    <circle cx="50" cy="85" r="25" fill="#ef4444" opacity="0.5"/>
    <circle cx="75" cy="85" r="25" fill="#3b82f6" opacity="0.5"/>
    <circle cx="90" cy="85" r="25" fill="#10b981" opacity="0.5"/>
    <text x="70" y="140" text-anchor="middle" font-size="8" fill="#64748b">los hijos se mezclan</text>
  </g>

  <!-- Caso 2: opacity en el grupo → se renderiza primero opaco y luego se aplica la opacidad -->
  <g opacity="0.5">
    <text x="220" y="40" text-anchor="middle" font-size="9" fill="#64748b" opacity="2">opacity en el &lt;g&gt;</text>
    <circle cx="200" cy="85" r="25" fill="#ef4444"/>
    <circle cx="225" cy="85" r="25" fill="#3b82f6"/>
    <circle cx="240" cy="85" r="25" fill="#10b981"/>
  </g>
  <text x="220" y="140" text-anchor="middle" font-size="8" fill="#64748b">grupo como una unidad</text>

  <text x="150" y="160" text-anchor="middle" font-size="8" fill="#94a3b8">
    opacity no se "hereda" — se aplica al grupo renderizado como capa
  </text>
</svg>
```

---

## Grupos anidados y jerarquía semántica

```svg
<svg width="300" height="170"
     viewBox="0 0 300 170"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">grupos anidados = jerarquía</text>

  <!-- Muñeco compuesto por subgrupos -->
  <g id="muneco" transform="translate(150, 95)" stroke="#1e293b" stroke-width="1.5">

    <!-- Subgrupo: piernas -->
    <g id="piernas" fill="#3b82f6">
      <rect x="-15" y="20" width="10" height="30"/>
      <rect x="5" y="20" width="10" height="30"/>
    </g>

    <!-- Subgrupo: torso -->
    <g id="torso" fill="#dbeafe">
      <rect x="-18" y="-15" width="36" height="40" rx="4"/>
    </g>

    <!-- Subgrupo: brazos -->
    <g id="brazos" fill="#fef9c3">
      <rect x="-32" y="-10" width="14" height="30" rx="3"/>
      <rect x="18" y="-10" width="14" height="30" rx="3"/>
    </g>

    <!-- Subgrupo: cabeza -->
    <g id="cabeza" fill="#fef9c3">
      <circle cx="0" cy="-28" r="14"/>
      <circle cx="-4" cy="-30" r="1.5" fill="#1e293b"/>
      <circle cx="4" cy="-30" r="1.5" fill="#1e293b"/>
    </g>
  </g>

  <text x="60"  y="50"  font-size="8" fill="#64748b">&lt;g id="muneco"&gt;</text>
  <text x="70"  y="65"  font-size="8" fill="#64748b">  &lt;g id="piernas"&gt;</text>
  <text x="70"  y="78"  font-size="8" fill="#64748b">  &lt;g id="torso"&gt;</text>
  <text x="70"  y="91"  font-size="8" fill="#64748b">  &lt;g id="brazos"&gt;</text>
  <text x="70"  y="104" font-size="8" fill="#64748b">  &lt;g id="cabeza"&gt;</text>
  <text x="60"  y="117" font-size="8" fill="#64748b">&lt;/g&gt;</text>

  <text x="150" y="160" text-anchor="middle" font-size="8" fill="#94a3b8">
    IDs descriptivos = SVG legible y manipulable desde JS/CSS
  </text>
</svg>
```
