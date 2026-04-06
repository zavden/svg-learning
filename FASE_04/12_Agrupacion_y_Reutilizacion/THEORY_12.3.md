# 12.3 El elemento `<symbol>`

`<symbol>` es la forma correcta de definir componentes reutilizables: como un `<g>` pero con su propio `viewBox` interno, igual que un `<svg>`. Es la base de cualquier sistema de iconos.

---

## `<symbol>` no se dibuja hasta ser referenciado

```svg
<svg width="300" height="130"
     viewBox="0 0 300 130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">&lt;symbol&gt; es invisible por sí solo</text>

  <!-- Este símbolo NO se renderiza aquí, está en defs esperando a ser invocado -->
  <defs>
    <symbol id="rayo" viewBox="0 0 24 24">
      <path d="M 13 2 L 4 14 L 12 14 L 10 22 L 20 10 L 12 10 Z"
            fill="#f59e0b" stroke="#92400e" stroke-width="1.5" stroke-linejoin="round"/>
    </symbol>
  </defs>

  <!-- Caja mostrando la definición como "dormida" -->
  <rect x="20" y="40" width="90" height="70" fill="#fef9c3" stroke="#f59e0b" stroke-width="1.5" stroke-dasharray="4,3" rx="3"/>
  <text x="65" y="55" text-anchor="middle" font-size="8" fill="#92400e">&lt;symbol&gt;</text>
  <text x="65" y="68" text-anchor="middle" font-size="7" fill="#64748b">id="rayo"</text>
  <text x="65" y="80" text-anchor="middle" font-size="7" fill="#64748b">viewBox="0 0 24 24"</text>
  <text x="65" y="95" text-anchor="middle" font-size="7" fill="#64748b" font-style="italic">no se ve</text>

  <!-- Al hacer <use>, el símbolo cobra vida -->
  <text x="140" y="78" text-anchor="middle" font-size="12">→</text>

  <use href="#rayo" x="170" y="45" width="60" height="60"/>
  <text x="200" y="120" text-anchor="middle" font-size="8" fill="#92400e">&lt;use&gt; lo invoca</text>
</svg>
```

---

## El `viewBox` propio: la diferencia clave con `<g>`

```svg
<svg width="300" height="170"
     viewBox="0 0 300 170"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">symbol = g + viewBox propio</text>

  <defs>
    <!-- Símbolo con viewBox 0 0 24 24: las coordenadas internas son pequeñas -->
    <symbol id="check" viewBox="0 0 24 24">
      <circle cx="12" cy="12" r="10" fill="#dcfce7" stroke="#10b981" stroke-width="2"/>
      <path d="M 7 12 L 11 16 L 17 9" fill="none" stroke="#10b981" stroke-width="3" stroke-linecap="round" stroke-linejoin="round"/>
    </symbol>
  </defs>

  <!-- El mismo símbolo renderizado en 3 tamaños diferentes -->
  <use href="#check" x="25"  y="45" width="30"  height="30"/>
  <text x="40"  y="95" text-anchor="middle" font-size="8" fill="#64748b">30×30</text>

  <use href="#check" x="80"  y="45" width="50"  height="50"/>
  <text x="105" y="110" text-anchor="middle" font-size="8" fill="#64748b">50×50</text>

  <use href="#check" x="150" y="35" width="80"  height="80"/>
  <text x="190" y="130" text-anchor="middle" font-size="8" fill="#64748b">80×80</text>

  <use href="#check" x="235" y="55" width="50"  height="50"/>
  <text x="260" y="120" text-anchor="middle" font-size="8" fill="#64748b">50×50</text>

  <text x="150" y="158" text-anchor="middle" font-size="8" fill="#94a3b8">
    viewBox interno → el símbolo se adapta al width/height del &lt;use&gt;
  </text>
</svg>
```

---

## Un mini sistema de iconos con `<symbol>`

```svg
<svg width="300" height="170"
     viewBox="0 0 300 170"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">biblioteca de iconos con &lt;symbol&gt;</text>

  <defs>
    <!-- Los tres iconos comparten viewBox 0 0 24 24 como estándar -->
    <symbol id="ic-home" viewBox="0 0 24 24">
      <path d="M 12 3 L 3 12 L 5 12 L 5 20 L 10 20 L 10 14 L 14 14 L 14 20 L 19 20 L 19 12 L 21 12 Z"
            fill="none" stroke="currentColor" stroke-width="2" stroke-linejoin="round"/>
    </symbol>

    <symbol id="ic-user" viewBox="0 0 24 24">
      <circle cx="12" cy="9" r="4" fill="none" stroke="currentColor" stroke-width="2"/>
      <path d="M 4 21 Q 4 14 12 14 Q 20 14 20 21"
            fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round"/>
    </symbol>

    <symbol id="ic-mail" viewBox="0 0 24 24">
      <rect x="3" y="5" width="18" height="14" rx="2" fill="none" stroke="currentColor" stroke-width="2"/>
      <path d="M 3 7 L 12 13 L 21 7" fill="none" stroke="currentColor" stroke-width="2" stroke-linejoin="round"/>
    </symbol>
  </defs>

  <!-- Cada icono en su "botón" con color controlado desde el style -->
  <g transform="translate(40, 60)">
    <rect x="-22" y="-22" width="60" height="60" rx="8" fill="#dbeafe"/>
    <use href="#ic-home" x="-16" y="-16" width="48" height="48" style="color:#1e40af"/>
  </g>
  <text x="48" y="120" text-anchor="middle" font-size="8" fill="#1e40af">home</text>

  <g transform="translate(140, 60)">
    <rect x="-22" y="-22" width="60" height="60" rx="8" fill="#dcfce7"/>
    <use href="#ic-user" x="-16" y="-16" width="48" height="48" style="color:#166534"/>
  </g>
  <text x="148" y="120" text-anchor="middle" font-size="8" fill="#166534">user</text>

  <g transform="translate(240, 60)">
    <rect x="-22" y="-22" width="60" height="60" rx="8" fill="#fce7f3"/>
    <use href="#ic-mail" x="-16" y="-16" width="48" height="48" style="color:#9d174d"/>
  </g>
  <text x="248" y="120" text-anchor="middle" font-size="8" fill="#9d174d">mail</text>

  <text x="150" y="155" text-anchor="middle" font-size="8" fill="#94a3b8">
    viewBox común + currentColor = sistema de iconos reutilizable
  </text>
</svg>
```

---

## `preserveAspectRatio` en `<symbol>`

```svg
<svg width="300" height="170"
     viewBox="0 0 300 170"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">preserveAspectRatio en &lt;symbol&gt;</text>

  <defs>
    <!-- Símbolo cuadrado 0 0 24 24 -->
    <symbol id="sq-meet" viewBox="0 0 24 24" preserveAspectRatio="xMidYMid meet">
      <rect x="2" y="2" width="20" height="20" fill="#dbeafe" stroke="#3b82f6" stroke-width="2"/>
      <circle cx="12" cy="12" r="7" fill="#3b82f6"/>
    </symbol>
    <symbol id="sq-none" viewBox="0 0 24 24" preserveAspectRatio="none">
      <rect x="2" y="2" width="20" height="20" fill="#dcfce7" stroke="#10b981" stroke-width="2"/>
      <circle cx="12" cy="12" r="7" fill="#10b981"/>
    </symbol>
  </defs>

  <!-- use con un rectángulo ancho: 120 x 50 -->
  <!-- Con meet (por defecto): preserva la proporción, deja espacio vacío -->
  <use href="#sq-meet" x="25" y="45" width="120" height="50"/>
  <rect x="25" y="45" width="120" height="50" fill="none" stroke="#94a3b8" stroke-dasharray="2,2"/>
  <text x="85" y="110" text-anchor="middle" font-size="8" fill="#3b82f6">meet (por defecto)</text>
  <text x="85" y="122" text-anchor="middle" font-size="7" fill="#64748b">conserva proporción</text>

  <!-- Con none: estira el contenido para rellenar -->
  <use href="#sq-none" x="165" y="45" width="120" height="50"/>
  <rect x="165" y="45" width="120" height="50" fill="none" stroke="#94a3b8" stroke-dasharray="2,2"/>
  <text x="225" y="110" text-anchor="middle" font-size="8" fill="#10b981">none</text>
  <text x="225" y="122" text-anchor="middle" font-size="7" fill="#64748b">estira para rellenar</text>

  <text x="150" y="150" text-anchor="middle" font-size="8" fill="#94a3b8">
    use "none" con cuidado: deforma el contenido
  </text>
</svg>
```

---

## Cuándo elegir `<symbol>` frente a `<g>`

```svg
<svg width="300" height="190"
     viewBox="0 0 300 190"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">&lt;symbol&gt; vs &lt;g&gt; — cuándo cada uno</text>

  <!-- Columna izquierda: <g> -->
  <rect x="15" y="35" width="130" height="140" fill="#fef9c3" stroke="#f59e0b" stroke-width="1.5" rx="4"/>
  <text x="80" y="52" text-anchor="middle" font-size="10" font-weight="bold" fill="#92400e">&lt;g&gt;</text>

  <text x="25" y="72"  font-size="7" fill="#1e293b">• agrupar elementos</text>
  <text x="25" y="85"  font-size="7" fill="#1e293b">• aplicar transform común</text>
  <text x="25" y="98"  font-size="7" fill="#1e293b">• heredar fill / stroke</text>
  <text x="25" y="111" font-size="7" fill="#1e293b">• jerarquía semántica</text>
  <text x="25" y="124" font-size="7" fill="#1e293b">• aplicar filtros a grupo</text>

  <text x="25" y="145" font-size="7" fill="#ef4444">× sin viewBox propio</text>
  <text x="25" y="158" font-size="7" fill="#ef4444">× sin escalado uniforme</text>

  <!-- Columna derecha: <symbol> -->
  <rect x="155" y="35" width="130" height="140" fill="#dbeafe" stroke="#3b82f6" stroke-width="1.5" rx="4"/>
  <text x="220" y="52" text-anchor="middle" font-size="10" font-weight="bold" fill="#1e40af">&lt;symbol&gt;</text>

  <text x="165" y="72"  font-size="7" fill="#1e293b">• componentes reutilizables</text>
  <text x="165" y="85"  font-size="7" fill="#1e293b">• iconos (el caso típico)</text>
  <text x="165" y="98"  font-size="7" fill="#1e293b">• viewBox independiente</text>
  <text x="165" y="111" font-size="7" fill="#1e293b">• width/height en &lt;use&gt;</text>
  <text x="165" y="124" font-size="7" fill="#1e293b">• sprites de iconos</text>

  <text x="165" y="145" font-size="7" fill="#ef4444">× no se dibuja solo</text>
  <text x="165" y="158" font-size="7" fill="#ef4444">× requiere &lt;use&gt;</text>
</svg>
```
