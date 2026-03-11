# 6.4 El elemento `<tspan>`

`<tspan>` es el equivalente SVG de `<span>` en HTML: permite estilizar partes del texto de forma diferente, o reposicionar fragmentos dentro de un `<text>`.

---

## Para qué sirve `<tspan>`

```svg
<svg width="300" height="110"
     viewBox="0 0 300 110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Cambiar estilo de una palabra -->
  <text x="20" y="30" font-size="15" fill="#1e293b">
    El precio es
    <tspan fill="#ef4444" font-weight="700">$99</tspan>
    hoy
  </text>

  <!-- Superíndice y subíndice -->
  <text x="20" y="65" font-size="15" fill="#1e293b">
    H<tspan dy="-5" font-size="10">2</tspan><tspan dy="5">O</tspan>
    <tspan dx="20">x<tspan dy="-6" font-size="10">2</tspan><tspan dy="6"> + y</tspan></tspan>
  </text>

  <!-- Resaltar parte de una etiqueta -->
  <text x="20" y="95" font-size="15" fill="#64748b">
    Temperatura:
    <tspan fill="#ef4444" font-weight="700" font-size="18">38.5</tspan>
    <tspan font-size="11">°C</tspan>
  </text>
</svg>
```

---

## Texto multilinea con `<tspan dy="...">`

La forma estándar de hacer texto en múltiples líneas en SVG. `dy="1.4em"` avanza una línea.

```svg
<svg width="300" height="130"
     viewBox="0 0 300 130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Tarjeta con texto multilinea -->
  <rect x="10" y="10" width="280" height="110" rx="8" fill="#0f172a"/>

  <text x="30" y="38" font-size="14" font-weight="700" fill="white">
    <tspan>Título del artículo</tspan>
    <tspan x="30" dy="1.5em" font-size="11" fill="#94a3b8" font-weight="400">Una descripción que ocupa</tspan>
    <tspan x="30" dy="1.4em" font-size="11" fill="#94a3b8">múltiples líneas de texto</tspan>
    <tspan x="30" dy="1.4em" font-size="11" fill="#94a3b8">usando tspan con dy</tspan>
  </text>

  <text x="150" y="122" text-anchor="middle" font-size="7" fill="#475569">
    tspan x="30" reinicia X; dy acumula el desplazamiento vertical
  </text>
</svg>
```

---

## `x`/`y` absoluto vs `dx`/`dy` relativo en `<tspan>`

```svg
<svg width="300" height="140"
     viewBox="0 0 300 140"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- x absoluto: se mueve a una posición exacta (interrumpe el flujo) -->
  <rect x="5" y="5" width="135" height="90" rx="4" fill="#f8fafc" stroke="#e2e8f0" stroke-width="1"/>
  <text x="20" y="32" font-size="13" fill="#1e293b">
    Hola
    <tspan x="70" fill="#3b82f6">mundo</tspan>
    !
  </text>
  <text x="20" y="55" font-size="7.5" fill="#64748b">tspan x=70: salta a x=70</text>
  <text x="20" y="70" font-size="7" fill="#94a3b8">el "!" continúa desde ahí</text>
  <line x1="70" y1="5" x2="70" y2="95" stroke="#3b82f6" stroke-width="1" stroke-dasharray="2,2"/>
  <text x="70" y="88" font-size="6.5" fill="#3b82f6">x=70</text>

  <!-- dx relativo: desplazamiento desde la posición natural -->
  <rect x="155" y="5" width="140" height="90" rx="4" fill="#f8fafc" stroke="#e2e8f0" stroke-width="1"/>
  <text x="170" y="32" font-size="13" fill="#1e293b">
    Hola
    <tspan dx="8" fill="#10b981">mundo</tspan>
    !
  </text>
  <text x="175" y="55" font-size="7.5" fill="#64748b">tspan dx=8: +8 desde aquí</text>
  <text x="175" y="70" font-size="7" fill="#94a3b8">el flujo continúa naturalmente</text>
</svg>
```

---

## `<tspan>` anidado

Los estilos se heredan del exterior al interior. El interior puede sobreescribir.

```svg
<svg width="300" height="80"
     viewBox="0 0 300 80"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <text x="20" y="35" font-size="16" fill="#1e293b">
    Normal
    <tspan fill="#3b82f6" font-weight="700">
      Azul bold
      <tspan font-style="italic" font-size="12">italic pequeño</tspan>
      bold
    </tspan>
    normal
  </text>
  <text x="20" y="60" font-size="7.5" fill="#64748b">tspan anidados: herencia de estilos</text>
  <text x="20" y="73" font-size="7" fill="#94a3b8">exterior → interior (puede sobreescribir)</text>
</svg>
```
