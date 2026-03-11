# Referencia — Transformaciones

---

## Cheat sheet de funciones

```svg
<svg width="300" height="190"
     viewBox="0 0 300 190"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">Funciones de transformación</text>

  <!-- translate -->
  <rect x="5" y="27" width="290" height="26" rx="2" fill="#dbeafe"/>
  <text x="12" y="39" font-size="8" fill="#1e40af" font-family="monospace" font-weight="600">translate(tx, ty)</text>
  <text x="12" y="49" font-size="7" fill="#3b82f6">Desplaza el sistema de coordenadas. ty=0 si se omite.</text>

  <!-- scale -->
  <rect x="5" y="57" width="290" height="26" rx="2" fill="#dcfce7"/>
  <text x="12" y="69" font-size="8" fill="#166534" font-family="monospace" font-weight="600">scale(sx, sy)</text>
  <text x="12" y="79" font-size="7" fill="#10b981">Escala desde (0,0). sy=sx si se omite. Negativo = espejo.</text>

  <!-- rotate -->
  <rect x="5" y="87" width="290" height="26" rx="2" fill="#fef9c3"/>
  <text x="12" y="99" font-size="8" fill="#92400e" font-family="monospace" font-weight="600">rotate(angle, cx, cy)</text>
  <text x="12" y="109" font-size="7" fill="#f59e0b">Rota en sentido horario. Sin cx,cy: desde (0,0).</text>

  <!-- skewX/Y -->
  <rect x="5" y="117" width="290" height="26" rx="2" fill="#fce7f3"/>
  <text x="12" y="129" font-size="8" fill="#9d174d" font-family="monospace" font-weight="600">skewX(a)  /  skewY(a)</text>
  <text x="12" y="139" font-size="7" fill="#ec4899">Inclina en el eje X o Y. Ángulos pequeños = perspectiva sutil.</text>

  <!-- matrix -->
  <rect x="5" y="147" width="290" height="26" rx="2" fill="#ede9fe"/>
  <text x="12" y="159" font-size="8" fill="#4c1d95" font-family="monospace" font-weight="600">matrix(a, b, c, d, e, f)</text>
  <text x="12" y="169" font-size="7" fill="#8b5cf6">Transformación directa en matriz. Para uso programático.</text>

  <text x="150" y="184" text-anchor="middle" font-size="7" fill="#94a3b8">
    todas se aplican al atributo transform="" del elemento o &lt;g&gt;
  </text>
</svg>
```

---

## Patrones de transformación comunes

```svg
<svg width="300" height="150"
     viewBox="0 0 300 150"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">Patrones frecuentes</text>

  <text x="10" y="36" font-size="7.5" fill="#64748b" font-weight="600">Rotar sobre el centro propio:</text>
  <text x="10" y="48" font-size="7" fill="#3b82f6" font-family="monospace">rotate(a, x+w/2, y+h/2)</text>
  <text x="10" y="58" font-size="6.5" fill="#94a3b8">o:  translate(cx,cy) rotate(a) translate(-cx,-cy)</text>

  <line x1="10" y1="64" x2="290" y2="64" stroke="#e2e8f0" stroke-width="1"/>

  <text x="10" y="77" font-size="7.5" fill="#64748b" font-weight="600">Escalar sobre el centro propio:</text>
  <text x="10" y="89" font-size="7" fill="#10b981" font-family="monospace">translate(cx,cy) scale(s) translate(-cx,-cy)</text>

  <line x1="10" y1="95" x2="290" y2="95" stroke="#e2e8f0" stroke-width="1"/>

  <text x="10" y="108" font-size="7.5" fill="#64748b" font-weight="600">Espejo horizontal (sin mover):</text>
  <text x="10" y="120" font-size="7" fill="#f59e0b" font-family="monospace">translate(2*cx, 0) scale(-1, 1)</text>

  <line x1="10" y1="126" x2="290" y2="126" stroke="#e2e8f0" stroke-width="1"/>

  <text x="10" y="139" font-size="7.5" fill="#64748b" font-weight="600">Posicionar elemento diseñado en (0,0):</text>
  <text x="10" y="150" font-size="7" fill="#8b5cf6" font-family="monospace">translate(x_final, y_final)</text>
</svg>
```

---

## Tabla de transformaciones vs matrix

| Función | matrix equivalente | Efecto |
|---|---|---|
| `translate(tx, ty)` | `matrix(1,0,0,1,tx,ty)` | Desplaza |
| `scale(sx, sy)` | `matrix(sx,0,0,sy,0,0)` | Escala |
| `scale(-1, 1)` | `matrix(-1,0,0,1,0,0)` | Espejo horizontal |
| `rotate(a)` | `matrix(cos,sin,-sin,cos,0,0)` | Rota desde (0,0) |
| `skewX(a)` | `matrix(1,0,tan(a),1,0,0)` | Inclina en X |
| `skewY(a)` | `matrix(1,tan(a),0,1,0,0)` | Inclina en Y |

---

## Regla de orden

```
transform="A B C"
→ El sistema se transforma primero con C, luego B, luego A
→ Desde la perspectiva del elemento: A se aplica primero visualmente

Ejemplo: translate(50,0) rotate(30)
→ El elemento se traslada 50px, LUEGO rota 30° alrededor del (nuevo) origen
```
