# 8.4 Diferencia entre `none` y `transparent`

Visualmente idénticos — ambos son invisibles. Pero se comportan de forma distinta con los eventos del mouse y los filtros. Confundirlos crea bugs interactivos sutiles.

---

## La diferencia visual... que no existe

```svg
<svg width="300" height="100"
     viewBox="0 0 300 100"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#94a3b8;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">Visualmente idénticos — comportamiento diferente</text>

  <!-- fill="none" -->
  <rect x="20" y="30" width="100" height="55" fill="none" stroke="white" stroke-width="2"/>
  <text x="70" y="72" text-anchor="middle" font-size="8.5" fill="white">fill="none"</text>
  <text x="70" y="90" text-anchor="middle" font-size="7" fill="#f1f5f9">sin pintura interior</text>

  <!-- fill="transparent" -->
  <rect x="180" y="30" width="100" height="55" fill="transparent" stroke="white" stroke-width="2"/>
  <text x="230" y="72" text-anchor="middle" font-size="8.5" fill="white">fill="transparent"</text>
  <text x="230" y="90" text-anchor="middle" font-size="7" fill="#f1f5f9">pintura alfa=0</text>
</svg>
```

---

## La diferencia real: eventos del mouse

Con `pointer-events` por defecto (`visiblePainted`), solo responden al mouse las zonas donde hay pintura visible:

- `fill="none"` → el interior **no responde** a hover/click
- `fill="transparent"` → el interior **sí responde** a hover/click (hay pintura, aunque invisible)

```svg
<svg width="300" height="110"
     viewBox="0 0 300 110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">pointer-events — el área de click</text>

  <!-- fill="none": solo el stroke responde -->
  <rect x="20" y="32" width="110" height="60" fill="none"
        stroke="#3b82f6" stroke-width="3"/>
  <text x="75" y="60" text-anchor="middle" font-size="7.5" fill="#64748b">fill="none"</text>
  <text x="75" y="73" text-anchor="middle" font-size="7" fill="#ef4444">✗ interior no clickeable</text>
  <text x="75" y="84" text-anchor="middle" font-size="7" fill="#94a3b8">solo el stroke responde</text>
  <text x="75" y="100" text-anchor="middle" font-size="7" fill="#64748b">(pointer-events default)</text>

  <!-- fill="transparent": todo el rect responde -->
  <rect x="170" y="32" width="110" height="60" fill="transparent"
        stroke="#10b981" stroke-width="3"/>
  <text x="225" y="60" text-anchor="middle" font-size="7.5" fill="#64748b">fill="transparent"</text>
  <text x="225" y="73" text-anchor="middle" font-size="7" fill="#10b981">✓ interior clickeable</text>
  <text x="225" y="84" text-anchor="middle" font-size="7" fill="#94a3b8">área invisible pero activa</text>
  <text x="225" y="100" text-anchor="middle" font-size="7" fill="#64748b">(pointer-events default)</text>
</svg>
```

---

## pointer-events modifica este comportamiento

```svg
<svg width="300" height="120"
     viewBox="0 0 300 120"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">pointer-events — control explícito</text>

  <!-- fill=none pero pointer-events=all: SÍ responde -->
  <rect x="10" y="32" width="85" height="55" fill="none" stroke="#3b82f6" stroke-width="2"
        pointer-events="all"/>
  <text x="52" y="60" text-anchor="middle" font-size="7" fill="#3b82f6">fill="none"</text>
  <text x="52" y="72" text-anchor="middle" font-size="7" fill="#10b981">pe="all"</text>
  <text x="52" y="82" text-anchor="middle" font-size="6.5" fill="#10b981">✓ clickeable</text>
  <text x="52" y="97" text-anchor="middle" font-size="6.5" fill="#94a3b8">pe overrides fill</text>

  <!-- fill=transparent pero pointer-events=none: NO responde -->
  <rect x="110" y="32" width="85" height="55" fill="transparent" stroke="#f59e0b" stroke-width="2"
        pointer-events="none"/>
  <text x="152" y="60" text-anchor="middle" font-size="7" fill="#f59e0b">fill="transparent"</text>
  <text x="152" y="72" text-anchor="middle" font-size="7" fill="#ef4444">pe="none"</text>
  <text x="152" y="82" text-anchor="middle" font-size="6.5" fill="#ef4444">✗ no clickeable</text>
  <text x="152" y="97" text-anchor="middle" font-size="6.5" fill="#94a3b8">pe bloquea fill</text>

  <!-- fill=none + pointer-events=none: no responde (normal) -->
  <rect x="210" y="32" width="85" height="55" fill="none" stroke="#94a3b8" stroke-width="2"
        pointer-events="none"/>
  <text x="252" y="60" text-anchor="middle" font-size="7" fill="#94a3b8">fill="none"</text>
  <text x="252" y="72" text-anchor="middle" font-size="7" fill="#94a3b8">pe="none"</text>
  <text x="252" y="82" text-anchor="middle" font-size="6.5" fill="#ef4444">✗ no clickeable</text>
  <text x="252" y="97" text-anchor="middle" font-size="6.5" fill="#94a3b8">ninguno responde</text>
</svg>
```

---

## Caso de uso: zona de click expandida

Un botón pequeño con zona de click más grande que el área visual:

```svg
<svg width="300" height="100"
     viewBox="0 0 300 100"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Zona de click visible (pequeña) -->
  <rect x="110" y="35" width="80" height="30" rx="6" fill="#3b82f6"/>
  <text x="150" y="54" text-anchor="middle" font-size="8" fill="white">Botón</text>

  <!-- Área de click expandida (invisible pero activa) -->
  <rect x="90" y="25" width="120" height="50" rx="8" fill="transparent"/>

  <!-- Anotación del área invisible -->
  <rect x="90" y="25" width="120" height="50" rx="8" fill="none"
        stroke="#ef4444" stroke-width="1" stroke-dasharray="3,2"/>
  <text x="150" y="87" text-anchor="middle" font-size="7" fill="#ef4444">área de click (transparent, invisible)</text>
  <text x="150" y="97" text-anchor="middle" font-size="6.5" fill="#94a3b8">20px de margen adicional alrededor del botón</text>
</svg>
```

---

## Tabla resumen

```svg
<svg width="300" height="110"
     viewBox="0 0 300 110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">none vs transparent</text>

  <!-- Cabeceras -->
  <text x="80"  y="34" text-anchor="middle" font-size="7.5" fill="#64748b" font-weight="700">fill="none"</text>
  <text x="210" y="34" text-anchor="middle" font-size="7.5" fill="#64748b" font-weight="700">fill="transparent"</text>
  <line x1="10" y1="37" x2="290" y2="37" stroke="#e2e8f0" stroke-width="1"/>

  <text x="15"  y="50" font-size="7" fill="#64748b">Apariencia</text>
  <text x="80"  y="50" text-anchor="middle" font-size="7" fill="#94a3b8">invisible</text>
  <text x="210" y="50" text-anchor="middle" font-size="7" fill="#94a3b8">invisible</text>

  <text x="15"  y="64" font-size="7" fill="#64748b">Hover/click</text>
  <text x="80"  y="64" text-anchor="middle" font-size="7" fill="#ef4444">NO (por defecto)</text>
  <text x="210" y="64" text-anchor="middle" font-size="7" fill="#10b981">SÍ (por defecto)</text>

  <text x="15"  y="78" font-size="7" fill="#64748b">Tiene "pintura"</text>
  <text x="80"  y="78" text-anchor="middle" font-size="7" fill="#94a3b8">No</text>
  <text x="210" y="78" text-anchor="middle" font-size="7" fill="#94a3b8">Sí (alfa=0)</text>

  <text x="15"  y="92" font-size="7" fill="#64748b">Caso de uso</text>
  <text x="80"  y="92" text-anchor="middle" font-size="7" fill="#64748b">solo contorno</text>
  <text x="210" y="92" text-anchor="middle" font-size="7" fill="#64748b">área click invisible</text>

  <text x="150" y="107" text-anchor="middle" font-size="6.5" fill="#94a3b8">
    pointer-events="all/none" overrides este comportamiento
  </text>
</svg>
```
