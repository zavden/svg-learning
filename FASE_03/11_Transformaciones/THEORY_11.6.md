# 11.6 `matrix(a, b, c, d, e, f)`

Todas las transformaciones SVG son internamente matrices. `matrix()` permite especificar cualquier transformación directamente, útil cuando se calculan programáticamente.

---

## Los seis parámetros

```svg
<svg width="300" height="110"
     viewBox="0 0 300 110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">matrix(a, b, c, d, e, f)</text>

  <!-- Representación visual de la matriz -->
  <rect x="60" y="30" width="180" height="55" rx="4" fill="#f1f5f9" stroke="#e2e8f0" stroke-width="1"/>

  <text x="90"  y="52" text-anchor="middle" font-size="10" fill="#3b82f6" font-family="monospace">a</text>
  <text x="130" y="52" text-anchor="middle" font-size="10" fill="#10b981" font-family="monospace">c</text>
  <text x="170" y="52" text-anchor="middle" font-size="10" fill="#f59e0b" font-family="monospace">e</text>

  <text x="90"  y="68" text-anchor="middle" font-size="10" fill="#3b82f6" font-family="monospace">b</text>
  <text x="130" y="68" text-anchor="middle" font-size="10" fill="#10b981" font-family="monospace">d</text>
  <text x="170" y="68" text-anchor="middle" font-size="10" fill="#f59e0b" font-family="monospace">f</text>

  <line x1="60" y1="57" x2="240" y2="57" stroke="#e2e8f0" stroke-width="0.5"/>
  <line x1="110" y1="30" x2="110" y2="85" stroke="#e2e8f0" stroke-width="0.5"/>
  <line x1="150" y1="30" x2="150" y2="85" stroke="#e2e8f0" stroke-width="0.5"/>

  <text x="90"  y="97" text-anchor="middle" font-size="6.5" fill="#3b82f6">escala/rot</text>
  <text x="150" y="97" text-anchor="middle" font-size="6.5" fill="#10b981">skew/rot</text>
  <text x="210" y="97" text-anchor="middle" font-size="6.5" fill="#f59e0b">translación</text>
</svg>
```

---

## Equivalencias con funciones individuales

```svg
<svg width="300" height="140"
     viewBox="0 0 300 140"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">Equivalencias — función vs matrix</text>

  <text x="10" y="36" font-size="7.5" fill="#3b82f6" font-family="monospace">translate(40, 20)</text>
  <text x="10" y="47" font-size="7" fill="#94a3b8" font-family="monospace">= matrix(1, 0, 0, 1, 40, 20)</text>

  <line x1="10" y1="53" x2="290" y2="53" stroke="#e2e8f0" stroke-width="0.5"/>

  <text x="10" y="66" font-size="7.5" fill="#10b981" font-family="monospace">scale(2, 3)</text>
  <text x="10" y="77" font-size="7" fill="#94a3b8" font-family="monospace">= matrix(2, 0, 0, 3, 0, 0)</text>

  <line x1="10" y1="83" x2="290" y2="83" stroke="#e2e8f0" stroke-width="0.5"/>

  <text x="10" y="96" font-size="7.5" fill="#f59e0b" font-family="monospace">rotate(30)</text>
  <text x="10" y="107" font-size="7" fill="#94a3b8" font-family="monospace">= matrix(0.866, 0.5, -0.5, 0.866, 0, 0)</text>

  <line x1="10" y1="113" x2="290" y2="113" stroke="#e2e8f0" stroke-width="0.5"/>

  <text x="10" y="126" font-size="7.5" fill="#8b5cf6" font-family="monospace">skewX(20)</text>
  <text x="10" y="137" font-size="7" fill="#94a3b8" font-family="monospace">= matrix(1, 0, tan(20°)≈0.364, 1, 0, 0)</text>
</svg>
```

---

## Cuándo usar `matrix` directamente

```svg
<svg width="300" height="100"
     viewBox="0 0 300 100"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">Cuándo usar matrix()</text>

  <text x="10" y="36" font-size="7.5" fill="#10b981">✓</text>
  <text x="22" y="36" font-size="7" fill="#64748b">JavaScript devuelve matrices (getCTM, DOMMatrix)</text>

  <text x="10" y="51" font-size="7.5" fill="#10b981">✓</text>
  <text x="22" y="51" font-size="7" fill="#64748b">Composición exacta calculada programáticamente</text>

  <text x="10" y="66" font-size="7.5" fill="#10b981">✓</text>
  <text x="22" y="66" font-size="7" fill="#64748b">Interpolar entre dos transformaciones complejas</text>

  <text x="10" y="81" font-size="7.5" fill="#ef4444">✗</text>
  <text x="22" y="81" font-size="7" fill="#64748b">SVG escrito a mano: preferir translate/rotate/scale (legible)</text>

  <text x="10" y="96" font-size="7.5" fill="#ef4444">✗</text>
  <text x="22" y="96" font-size="7" fill="#64748b">matrix(0.866, 0.5, -0.5, 0.866, 0, 0) no es intuitivo</text>
</svg>
```
