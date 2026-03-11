# 5.10 `fill-rule` en paths

Cuando un path tiene sub-trazados superpuestos o contornos que se cruzan, `fill-rule` define qué regiones se consideran "dentro" del path.

---

## El problema: ambigüedad de interior

```svg
<svg width="300" height="110"
     viewBox="0 0 300 110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Estrella de 5 puntas: las regiones internas se solapan -->
  <!-- ¿Qué está "dentro"? Todo, o solo las puntas? -->
  <path d="M 75 10 L 91 57 H 40 L 83 85 L 57 130 L 75 85 L 93 130 L 67 85 L 110 57 H 59 Z"
        stroke="#64748b" stroke-width="1.5" fill="#dbeafe"/>
  <text x="75" y="108" text-anchor="middle" font-size="8" fill="#64748b">¿todo es interior?</text>

  <!-- Anillo: ¿el hueco central está dentro o fuera? -->
  <path d="M 200 55 m -45 0 a 45 45 0 1 1 90 0 a 45 45 0 1 1 -90 0
           M 200 55 m -22 0 a 22 22 0 1 1 44 0 a 22 22 0 1 1 -44 0"
        stroke="#64748b" stroke-width="1.5" fill="#dbeafe"/>
  <text x="200" y="108" text-anchor="middle" font-size="8" fill="#64748b">¿el centro es exterior?</text>
</svg>
```

---

## `fill-rule="nonzero"` (valor por defecto)

Traza un rayo desde cada punto. Cada cruce en sentido horario suma +1, antihorario –1. Si el resultado ≠ 0 → interior.

```svg
<svg width="300" height="130"
     viewBox="0 0 300 130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Estrella nonzero: todo se rellena -->
  <path d="M 75 10 L 86.76 45.27 H 123.53 L 94.39 66.18 L 105.71 101.45 L 75 80.71 L 44.29 101.45 L 55.61 66.18 L 26.47 45.27 H 63.24 Z"
        fill="#3b82f6" fill-rule="nonzero" opacity="0.85"/>
  <text x="75" y="118" text-anchor="middle" font-size="7.5" fill="#64748b">nonzero</text>
  <text x="75" y="129" text-anchor="middle" font-size="7" fill="#94a3b8">todo relleno</text>

  <!-- Anillo nonzero: ambos círculos van en el mismo sentido → no hay agujero -->
  <path d="M 195 55 m -50 0 a 50 50 0 1 1 100 0 a 50 50 0 1 1 -100 0
           M 195 55 m -25 0 a 25 25 0 1 1 50 0 a 25 25 0 1 1 -50 0"
        fill="#3b82f6" fill-rule="nonzero" opacity="0.85"/>
  <text x="195" y="118" text-anchor="middle" font-size="7.5" fill="#64748b">nonzero</text>
  <text x="195" y="129" text-anchor="middle" font-size="7" fill="#94a3b8">sin agujero (mismo sentido)</text>
</svg>
```

---

## `fill-rule="evenodd"`

Traza un rayo desde cada punto. Si el número de cruces es **impar** → interior; si es **par** → exterior.

```svg
<svg width="300" height="130"
     viewBox="0 0 300 130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Estrella evenodd: centro queda vacío -->
  <path d="M 75 10 L 86.76 45.27 H 123.53 L 94.39 66.18 L 105.71 101.45 L 75 80.71 L 44.29 101.45 L 55.61 66.18 L 26.47 45.27 H 63.24 Z"
        fill="#3b82f6" fill-rule="evenodd" opacity="0.85"/>
  <text x="75" y="118" text-anchor="middle" font-size="7.5" fill="#64748b">evenodd</text>
  <text x="75" y="129" text-anchor="middle" font-size="7" fill="#94a3b8">centro vacío</text>

  <!-- Anillo evenodd: el interior se vacía (2 cruces = par = exterior) -->
  <path d="M 195 55 m -50 0 a 50 50 0 1 1 100 0 a 50 50 0 1 1 -100 0
           M 195 55 m -25 0 a 25 25 0 1 1 50 0 a 25 25 0 1 1 -50 0"
        fill="#3b82f6" fill-rule="evenodd" opacity="0.85"/>
  <text x="195" y="118" text-anchor="middle" font-size="7.5" fill="#64748b">evenodd</text>
  <text x="195" y="129" text-anchor="middle" font-size="7" fill="#94a3b8">agujero ✓</text>
</svg>
```

---

## Patrón para crear agujeros con `evenodd`

```svg
<svg width="300" height="130"
     viewBox="0 0 300 130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Donut -->
  <path d="M 60 65
           m -50 0
           a 50 50 0 1 1 100 0
           a 50 50 0 1 1 -100 0
           M 60 65
           m -25 0
           a 25 25 0 1 1 50 0
           a 25 25 0 1 1 -50 0"
        fill="#f59e0b" fill-rule="evenodd" opacity="0.9"/>
  <text x="60" y="122" text-anchor="middle" font-size="7.5" fill="#64748b">donut</text>

  <!-- Marco / ventana -->
  <path d="M 130 15 H 270 V 105 H 130 Z
           M 148 33 H 252 V 87 H 148 Z"
        fill="#6366f1" fill-rule="evenodd" opacity="0.8"/>
  <text x="200" y="120" text-anchor="middle" font-size="7.5" fill="#64748b">marco / ventana</text>

  <!-- Código esquemático -->
  <text x="200" y="8" text-anchor="middle" font-size="7" fill="#94a3b8">
    exterior Z + interior Z + fill-rule="evenodd"
  </text>
</svg>
```

---

## Tabla resumen

```svg
<svg width="300" height="100"
     viewBox="0 0 300 100"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">Cuándo usar cada fill-rule</text>

  <text x="10" y="38" font-size="8" fill="#3b82f6" font-weight="700">nonzero (defecto)</text>
  <text x="10" y="51" font-size="7" fill="#64748b">• Formas simples sin cruces</text>
  <text x="10" y="62" font-size="7" fill="#64748b">• Paths generados por editores gráficos</text>
  <text x="10" y="73" font-size="7" fill="#64748b">• No cambiarlo si no sabes el sentido</text>

  <line x1="155" y1="22" x2="155" y2="100" stroke="#e2e8f0" stroke-width="1"/>

  <text x="165" y="38" font-size="8" fill="#6366f1" font-weight="700">evenodd</text>
  <text x="165" y="51" font-size="7" fill="#64748b">• Crear agujeros: donut, marco, letra "O"</text>
  <text x="165" y="62" font-size="7" fill="#64748b">• Estrellas con centro hueco</text>
  <text x="165" y="73" font-size="7" fill="#64748b">• No importa el sentido del sub-trazado</text>
</svg>
```
