# 24.5 Librerías JavaScript para SVG

A partir de cierto tamaño, manipular SVG a pelo (createElementNS, setAttribute) deja de escalar. Las **librerías especializadas** resuelven las partes aburridas: D3 para datos, GSAP para animación de alta precisión, Lottie para After Effects en el navegador, Vivus para line-drawing, SVG.js para DOM fluido. Cada una cubre un nicho — elegir bien depende del problema, no de la popularidad.

---

## D3.js — el estándar de la visualización de datos

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    D3: datos ↔ DOM
  </text>

  <rect x="15" y="38" width="270" height="80" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#166534">Filosofía</text>
  <text x="25" y="70" font-size="7" fill="#475569">no dibuja: une datos a elementos del DOM.</text>
  <text x="25" y="84" font-size="7" fill="#475569">cada dato → un elemento → cada update reutiliza</text>
  <text x="25" y="98" font-size="7" fill="#475569">o crea/elimina según entra, update, exit</text>

  <rect x="15" y="126" width="270" height="88" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="142" font-size="9" font-weight="bold" fill="#1e40af">Casos ideales</text>
  <text x="25" y="158" font-size="7" fill="#475569">• gráficos estadísticos (barras, áreas, dispersión)</text>
  <text x="25" y="172" font-size="7" fill="#475569">• mapas geográficos con GeoJSON y proyecciones</text>
  <text x="25" y="186" font-size="7" fill="#475569">• jerarquías (árbol, treemap, force-directed)</text>
  <text x="25" y="200" font-size="7" fill="#475569">• cualquier viz donde los datos dicten la forma</text>

  <rect x="15" y="222" width="270" height="50" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="238" font-size="9" font-weight="bold" fill="#92400e">Ojo</text>
  <text x="25" y="254" font-size="7" fill="#475569">curva empinada. Para iconos o animaciones simples,</text>
  <text x="25" y="264" font-size="7" fill="#475569">D3 es un cañón para matar una mosca</text>
</svg>
```

---

## GSAP — animación de alta precisión

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    la librería de animación de referencia
  </text>

  <rect x="15" y="38" width="270" height="96" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#166534">Qué aporta</text>
  <text x="25" y="70" font-size="7" fill="#475569">• anima cualquier atributo SVG o propiedad CSS</text>
  <text x="25" y="84" font-size="7" fill="#475569">• timeline sofisticado para secuencias complejas</text>
  <text x="25" y="98" font-size="7" fill="#475569">• plugins: DrawSVG, MorphSVG, MotionPath</text>
  <text x="25" y="112" font-size="7" fill="#475569">• rendimiento excepcional, batch updates</text>
  <text x="25" y="126" font-size="7" fill="#166534">→ estándar de animación web profesional</text>

  <rect x="15" y="142" width="270" height="82" fill="#0f172a" rx="3"/>
  <text x="25" y="158" font-size="7" font-family="monospace" fill="#94a3b8">// ejemplo mínimo</text>
  <text x="25" y="172" font-size="8" font-family="monospace" fill="#60a5fa">import { gsap } from 'gsap'</text>
  <text x="25" y="192" font-size="8" font-family="monospace" fill="#34d399">gsap.to('.box', {</text>
  <text x="25" y="206" font-size="8" font-family="monospace" fill="#34d399">  x: 100, rotation: 360,</text>
  <text x="25" y="220" font-size="8" font-family="monospace" fill="#34d399">  duration: 2, ease: 'power2.out'</text>

  <rect x="15" y="234" width="270" height="38" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="250" font-size="9" font-weight="bold" fill="#92400e">Licencia</text>
  <text x="25" y="264" font-size="7" fill="#475569">core gratuito; plugins MorphSVG etc. = Club GSAP</text>
</svg>
```

---

## Anime.js — alternativa ligera

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    ~17KB vs ~70KB de GSAP core
  </text>

  <rect x="15" y="38" width="270" height="88" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#1e40af">Cuándo elegirla</text>
  <text x="25" y="70" font-size="7" fill="#475569">• bundle size importa (landing pages)</text>
  <text x="25" y="84" font-size="7" fill="#475569">• animaciones moderadamente complejas</text>
  <text x="25" y="98" font-size="7" fill="#475569">• no necesitas plugins avanzados de GSAP</text>
  <text x="25" y="112" font-size="7" fill="#475569">• API intuitiva y directa</text>

  <rect x="15" y="134" width="270" height="100" fill="#0f172a" rx="3"/>
  <text x="25" y="150" font-size="7" font-family="monospace" fill="#94a3b8">// ejemplo</text>
  <text x="25" y="164" font-size="8" font-family="monospace" fill="#60a5fa">import anime from 'animejs'</text>
  <text x="25" y="184" font-size="8" font-family="monospace" fill="#34d399">anime({</text>
  <text x="25" y="198" font-size="8" font-family="monospace" fill="#34d399">  targets: '.circle',</text>
  <text x="25" y="212" font-size="8" font-family="monospace" fill="#34d399">  translateX: 250,</text>
  <text x="25" y="226" font-size="8" font-family="monospace" fill="#34d399">  easing: 'easeInOutQuad'</text>
</svg>
```

---

## Vivus.js — line drawing puro

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    una cosa, bien hecha
  </text>

  <rect x="15" y="38" width="270" height="84" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#166534">Qué hace</text>
  <text x="25" y="70" font-size="7" fill="#475569">anima stroke-dashoffset en cada &lt;path&gt;</text>
  <text x="25" y="84" font-size="7" fill="#475569">de 0 a la longitud total del path</text>
  <text x="25" y="98" font-size="7" fill="#475569">→ efecto: el SVG se "dibuja solo"</text>
  <text x="25" y="112" font-size="7" fill="#166534">elegante para logos, ilustraciones lineales</text>

  <rect x="15" y="130" width="270" height="104" fill="#0f172a" rx="3"/>
  <text x="25" y="146" font-size="7" font-family="monospace" fill="#94a3b8">// setup mínimo</text>
  <text x="25" y="162" font-size="8" font-family="monospace" fill="#60a5fa">new Vivus('logo', {</text>
  <text x="25" y="176" font-size="8" font-family="monospace" fill="#34d399">  duration: 200,</text>
  <text x="25" y="190" font-size="8" font-family="monospace" fill="#34d399">  type: 'delayed'</text>
  <text x="25" y="204" font-size="8" font-family="monospace" fill="#60a5fa">})</text>
  <text x="25" y="224" font-size="7" fill="#fbbf24">→ requiere SVG con paths y stroke (sin fill)</text>
</svg>
```

---

## Lottie — After Effects en el navegador

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    animaciones complejas sin picar código
  </text>

  <rect x="15" y="38" width="270" height="82" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#166534">Cómo funciona</text>
  <text x="25" y="70" font-size="7" fill="#475569">1. animador trabaja en After Effects</text>
  <text x="25" y="84" font-size="7" fill="#475569">2. plugin Bodymovin exporta a .json</text>
  <text x="25" y="98" font-size="7" fill="#475569">3. lottie-web renderiza el json en el browser</text>
  <text x="25" y="112" font-size="7" fill="#166534">el renderer SVG mantiene la calidad vectorial</text>

  <rect x="15" y="128" width="270" height="82" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="144" font-size="9" font-weight="bold" fill="#1e40af">Casos ideales</text>
  <text x="25" y="160" font-size="7" fill="#475569">• onboarding animations de apps móviles</text>
  <text x="25" y="174" font-size="7" fill="#475569">• loading states con personalidad</text>
  <text x="25" y="188" font-size="7" fill="#475569">• iconos animados complejos (menús, likes)</text>
  <text x="25" y="202" font-size="7" fill="#475569">• equipos con animadores AE trabajando</text>

  <rect x="15" y="218" width="270" height="54" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="234" font-size="9" font-weight="bold" fill="#92400e">Precio a pagar</text>
  <text x="25" y="250" font-size="7" fill="#475569">• lottie-web ~250KB (y el json puede ser grande)</text>
  <text x="25" y="262" font-size="7" fill="#475569">• dotLottie es un formato más compacto</text>
</svg>
```

---

## SVG.js, Snap.svg y Paper.js

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    manipulación con API fluida
  </text>

  <rect x="15" y="38" width="270" height="68" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#1e40af">SVG.js (~13KB)</text>
  <text x="25" y="70" font-size="7" fill="#475569">crear y manipular SVG con chaining fluido,</text>
  <text x="25" y="84" font-size="7" fill="#475569">animación básica, mantenida activamente</text>
  <text x="25" y="98" font-size="7" fill="#475569">→ jQuery para SVG, más simple que D3</text>

  <rect x="15" y="114" width="270" height="60" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="130" font-size="9" font-weight="bold" fill="#92400e">Snap.svg</text>
  <text x="25" y="146" font-size="7" fill="#475569">creada por Adobe, diseño similar a SVG.js</text>
  <text x="25" y="160" font-size="7" fill="#475569">menos mantenida — prefiere SVG.js en proyectos nuevos</text>

  <rect x="15" y="182" width="270" height="68" fill="#ede9fe" stroke="#8b5cf6" rx="3"/>
  <text x="25" y="198" font-size="9" font-weight="bold" fill="#5b21b6">Paper.js</text>
  <text x="25" y="214" font-size="7" fill="#475569">framework para Canvas con modelo vectorial,</text>
  <text x="25" y="228" font-size="7" fill="#475569">geometría computacional, apps de dibujo</text>
  <text x="25" y="242" font-size="7" fill="#475569">→ usar cuando se necesita el render de Canvas</text>
</svg>
```
