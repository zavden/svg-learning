# 22.5 Organización de un sistema de iconos

Un sistema de iconos no es solo "una carpeta con SVGs". Para que escale: convención de nombres, tamaño base coherente, `currentColor` por defecto, y un catálogo donde cualquiera pueda encontrar el icono que busca. Estas decisiones se toman al inicio — cambiarlas después duele.

---

## Convención de naming para IDs

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    prefijo + nombre semántico
  </text>

  <rect x="15" y="38" width="270" height="92" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#166534">Patrones aceptables</text>
  <text x="25" y="70" font-size="7" font-family="monospace" fill="#475569">icon-home, icon-user, icon-close</text>
  <text x="25" y="84" font-size="7" font-family="monospace" fill="#475569">ic-arrow-right, ic-arrow-left</text>
  <text x="25" y="98" font-size="7" font-family="monospace" fill="#475569">i-chevron-down (muy corto)</text>
  <text x="25" y="114" font-size="7" fill="#166534">→ lo importante es ser consistente en todo el repo</text>

  <rect x="15" y="138" width="270" height="60" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="154" font-size="9" font-weight="bold" fill="#1e40af">Variantes del mismo icono</text>
  <text x="25" y="170" font-size="7" font-family="monospace" fill="#475569">icon-arrow-right / -left / -up / -down</text>
  <text x="25" y="184" font-size="7" font-family="monospace" fill="#475569">icon-heart, icon-heart-filled, icon-heart-outline</text>

  <rect x="15" y="206" width="270" height="60" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="222" font-size="9" font-weight="bold" fill="#991b1b">Evitar</text>
  <text x="25" y="238" font-size="7" font-family="monospace" fill="#475569">id="my icon"   ← espacio inválido</text>
  <text x="25" y="252" font-size="7" font-family="monospace" fill="#475569">id="24-home"   ← empezar por número</text>
</svg>
```

---

## Tamaño base estándar

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    elige un viewBox y sé consistente
  </text>

  <rect x="10" y="32" width="280" height="20" fill="#e2e8f0"/>
  <text x="20" y="46" font-size="7" font-weight="bold" fill="#1e293b">viewBox</text>
  <text x="140" y="46" font-size="7" font-weight="bold" fill="#1e293b">uso típico</text>

  <rect x="10" y="52" width="280" height="22" fill="white"/>
  <text x="20" y="66" font-size="7" font-family="monospace" fill="#3b82f6">0 0 16 16</text>
  <text x="140" y="66" font-size="7" fill="#475569">muy pequeño, inline en texto</text>

  <rect x="10" y="74" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="88" font-size="7" font-family="monospace" fill="#3b82f6">0 0 24 24</text>
  <text x="140" y="88" font-size="7" fill="#166534">estándar de facto ← recomendado</text>

  <rect x="10" y="96" width="280" height="22" fill="white"/>
  <text x="20" y="110" font-size="7" font-family="monospace" fill="#3b82f6">0 0 32 32</text>
  <text x="140" y="110" font-size="7" fill="#475569">tamaño medio</text>

  <rect x="10" y="118" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="132" font-size="7" font-family="monospace" fill="#3b82f6">0 0 48 48</text>
  <text x="140" y="132" font-size="7" fill="#475569">grande, ilustrativo</text>

  <rect x="15" y="154" width="270" height="90" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="170" font-size="9" font-weight="bold" fill="#92400e">Por qué 24×24 es el estándar</text>
  <text x="25" y="184" font-size="7" fill="#475569">• Material Design, Heroicons, Lucide, Tabler…</text>
  <text x="25" y="198" font-size="7" fill="#475569">• permite trazos de 1.5-2px bien definidos</text>
  <text x="25" y="212" font-size="7" fill="#475569">• escala a 16, 20, 24, 32 sin artefactos</text>
  <text x="25" y="226" font-size="7" fill="#92400e">elegir otra base es posible, pero te desconecta</text>
  <text x="25" y="238" font-size="7" fill="#92400e">de librerías existentes — no mezcles 24 y 20</text>
</svg>
```

---

## Sistema de tamaños CSS

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    misma base, muchos tamaños
  </text>

  <rect x="15" y="38" width="270" height="170" fill="#0f172a" rx="3"/>
  <text x="25" y="56" font-size="7" font-family="monospace" fill="#94a3b8">/* con viewBox="0 0 24 24" como base */</text>
  <text x="25" y="74" font-size="8" font-family="monospace" fill="#60a5fa">.icon-sm  {</text>
  <text x="25" y="88" font-size="8" font-family="monospace" fill="#34d399">  width: 16px; height: 16px;</text>
  <text x="25" y="102" font-size="8" font-family="monospace" fill="#60a5fa">}</text>
  <text x="25" y="116" font-size="8" font-family="monospace" fill="#fbbf24">.icon-md  { width: 24px; height: 24px; }</text>
  <text x="25" y="130" font-size="8" font-family="monospace" fill="#fbbf24">.icon-lg  { width: 32px; height: 32px; }</text>
  <text x="25" y="144" font-size="8" font-family="monospace" fill="#fbbf24">.icon-xl  { width: 48px; height: 48px; }</text>
  <text x="25" y="164" font-size="7" fill="#94a3b8">el mismo SVG rinde bien en todos los tamaños</text>
  <text x="25" y="178" font-size="7" fill="#94a3b8">gracias al viewBox interno</text>
  <text x="25" y="196" font-size="7" fill="#fbbf24">alternativa: width/height en em, heredando del texto</text>
</svg>
```

---

## `currentColor` por defecto

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    dos estrategias, misma idea
  </text>

  <rect x="15" y="38" width="270" height="92" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">// 1) en el propio símbolo</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#60a5fa">&lt;symbol id="icon-home" viewBox="0 0 24 24"&gt;</text>
  <text x="25" y="84" font-size="8" font-family="monospace" fill="#34d399">  &lt;path fill="currentColor" d="..."/&gt;</text>
  <text x="25" y="98" font-size="8" font-family="monospace" fill="#60a5fa">&lt;/symbol&gt;</text>
  <text x="25" y="118" font-size="7" fill="#94a3b8">→ el color del símbolo ya depende del contexto</text>

  <rect x="15" y="138" width="270" height="110" fill="#0f172a" rx="3"/>
  <text x="25" y="154" font-size="7" font-family="monospace" fill="#94a3b8">// 2) en el contenedor del &lt;use&gt;</text>
  <text x="25" y="170" font-size="8" font-family="monospace" fill="#60a5fa">.icono { fill: currentColor; }</text>
  <text x="25" y="192" font-size="7" font-family="monospace" fill="#94a3b8">// uso</text>
  <text x="25" y="208" font-size="8" font-family="monospace" fill="#fbbf24">&lt;button style="color: #3b82f6"&gt;</text>
  <text x="25" y="222" font-size="8" font-family="monospace" fill="#fbbf24">  &lt;svg class="icono"&gt;&lt;use href="#home"/&gt;</text>
  <text x="25" y="236" font-size="8" font-family="monospace" fill="#fbbf24">&lt;/svg&gt;</text>
</svg>
```

---

## Iconos bicolor con variables CSS

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    dos colores independientes
  </text>

  <rect x="15" y="38" width="270" height="96" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">&lt;!-- símbolo con dos var CSS --&gt;</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#60a5fa">&lt;symbol id="icon-mail"&gt;</text>
  <text x="25" y="86" font-size="8" font-family="monospace" fill="#34d399">  &lt;path fill="var(--bg, #e2e8f0)" d="..."/&gt;</text>
  <text x="25" y="100" font-size="8" font-family="monospace" fill="#34d399">  &lt;path fill="var(--fg, #3b82f6)" d="..."/&gt;</text>
  <text x="25" y="114" font-size="8" font-family="monospace" fill="#60a5fa">&lt;/symbol&gt;</text>

  <rect x="15" y="142" width="270" height="104" fill="#0f172a" rx="3"/>
  <text x="25" y="158" font-size="7" font-family="monospace" fill="#94a3b8">/* tematizar desde fuera */</text>
  <text x="25" y="174" font-size="8" font-family="monospace" fill="#60a5fa">.tema-claro {</text>
  <text x="25" y="188" font-size="8" font-family="monospace" fill="#fbbf24">  --bg: #f1f5f9;</text>
  <text x="25" y="202" font-size="8" font-family="monospace" fill="#fbbf24">  --fg: #1e293b;</text>
  <text x="25" y="216" font-size="8" font-family="monospace" fill="#60a5fa">}</text>
  <text x="25" y="236" font-size="7" fill="#94a3b8">→ tres iconos x 5 temas = 3 archivos, no 15</text>
</svg>
```

---

## Documentación del sistema

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    un sistema sin catálogo no es un sistema
  </text>

  <rect x="15" y="38" width="270" height="190" fill="#0f172a" rx="3"/>
  <text x="25" y="58" font-size="8" font-weight="bold" fill="#94a3b8">Qué debe existir</text>

  <text x="25" y="78" font-size="7" fill="#e2e8f0">• catálogo visual con todos los iconos e IDs</text>
  <text x="25" y="92" font-size="7" fill="#e2e8f0">  (Storybook, página MDX, demo estática)</text>

  <text x="25" y="112" font-size="7" fill="#e2e8f0">• guía de uso: import, props, temas, tamaños</text>

  <text x="25" y="132" font-size="7" fill="#e2e8f0">• política de contribución: tamaño, estilo,</text>
  <text x="25" y="146" font-size="7" fill="#e2e8f0">  convención de nombre, aprobación</text>

  <text x="25" y="166" font-size="7" fill="#e2e8f0">• changelog: iconos añadidos, renombrados,</text>
  <text x="25" y="180" font-size="7" fill="#e2e8f0">  deprecated (con fecha de eliminación)</text>

  <text x="25" y="202" font-size="7" fill="#fbbf24">→ la documentación es la diferencia entre</text>
  <text x="25" y="216" font-size="7" fill="#fbbf24">   "una carpeta de SVGs" y "un sistema"</text>
</svg>
```
