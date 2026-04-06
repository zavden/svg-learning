# 25.8 Guía de decisión rápida

Si ya llegaste hasta aquí, sabes el "cómo" del SVG. Esta última sección es el **cuándo**: cuándo elegir SVG vs otra cosa, qué librería usar para qué problema, y qué caminos tomar si quieres ir más allá de este roadmap.

---

## SVG vs alternativas

```svg
<svg width="300" height="320"
     viewBox="0 0 300 320"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    cada caso, su herramienta
  </text>

  <rect x="10" y="32" width="280" height="20" fill="#e2e8f0"/>
  <text x="20" y="46" font-size="7" font-weight="bold" fill="#1e293b">Caso</text>
  <text x="170" y="46" font-size="7" font-weight="bold" fill="#1e293b">Elige</text>

  <rect x="10" y="52" width="280" height="22" fill="white"/>
  <text x="20" y="66" font-size="7" fill="#475569">Icono monocromo</text>
  <text x="170" y="66" font-size="7" fill="#3b82f6">SVG + currentColor</text>

  <rect x="10" y="74" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="88" font-size="7" fill="#475569">Icono multicolor tematizable</text>
  <text x="170" y="88" font-size="7" fill="#3b82f6">SVG + CSS vars</text>

  <rect x="10" y="96" width="280" height="22" fill="white"/>
  <text x="20" y="110" font-size="7" fill="#475569">Logo de empresa</text>
  <text x="170" y="110" font-size="7" fill="#3b82f6">SVG (escalable ∞)</text>

  <rect x="10" y="118" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="132" font-size="7" fill="#475569">Fotografía</text>
  <text x="170" y="132" font-size="7" fill="#ef4444">JPEG/WebP (no SVG)</text>

  <rect x="10" y="140" width="280" height="22" fill="white"/>
  <text x="20" y="154" font-size="7" fill="#475569">Ilustración fotorrealista</text>
  <text x="170" y="154" font-size="7" fill="#ef4444">PNG/WebP (no SVG)</text>

  <rect x="10" y="162" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="176" font-size="7" fill="#475569">Gráfico con datos dinámicos</text>
  <text x="170" y="176" font-size="7" fill="#3b82f6">SVG + JS o D3</text>

  <rect x="10" y="184" width="280" height="22" fill="white"/>
  <text x="20" y="198" font-size="7" fill="#475569">Animación elaborada (AE)</text>
  <text x="170" y="198" font-size="7" fill="#3b82f6">Lottie</text>

  <rect x="10" y="206" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="220" font-size="7" fill="#475569">Logo que se dibuja solo</text>
  <text x="170" y="220" font-size="7" fill="#3b82f6">SVG + Vivus o CSS</text>

  <rect x="10" y="228" width="280" height="22" fill="white"/>
  <text x="20" y="242" font-size="7" fill="#475569">Fondo decorativo</text>
  <text x="170" y="242" font-size="7" fill="#3b82f6">SVG o CSS gradients</text>

  <rect x="10" y="250" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="264" font-size="7" fill="#475569">Patrón repetible</text>
  <text x="170" y="264" font-size="7" fill="#3b82f6">&lt;pattern&gt; o CSS</text>

  <rect x="10" y="272" width="280" height="22" fill="white"/>
  <text x="20" y="286" font-size="7" fill="#475569">Miles de elementos</text>
  <text x="170" y="286" font-size="7" fill="#ef4444">Canvas / WebGL</text>

  <rect x="10" y="294" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="308" font-size="7" fill="#475569">3D con luces y texturas</text>
  <text x="170" y="308" font-size="7" fill="#ef4444">Three.js / WebGPU</text>
</svg>
```

---

## Cuándo SVG es la respuesta equivocada

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    las 3 señales de que no debes usar SVG
  </text>

  <rect x="15" y="38" width="270" height="68" fill="#fee2e2" stroke="#ef4444" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#991b1b">1. Imagen continua</text>
  <text x="25" y="70" font-size="7" fill="#475569">fotografías, texturas, gradientes complejos</text>
  <text x="25" y="84" font-size="7" fill="#475569">SVG sería un archivo gigantesco comparado</text>
  <text x="25" y="98" font-size="7" fill="#475569">con WebP o AVIF de la misma calidad visual</text>

  <rect x="15" y="114" width="270" height="68" fill="#fee2e2" stroke="#ef4444" rx="3"/>
  <text x="25" y="130" font-size="9" font-weight="bold" fill="#991b1b">2. Escala masiva de elementos</text>
  <text x="25" y="146" font-size="7" fill="#475569">más de 5.000 nodos DOM degrada cualquier</text>
  <text x="25" y="160" font-size="7" fill="#475569">navegador. Un scatter plot con 100k puntos</text>
  <text x="25" y="174" font-size="7" fill="#475569">necesita Canvas o WebGL, no SVG</text>

  <rect x="15" y="190" width="270" height="80" fill="#fee2e2" stroke="#ef4444" rx="3"/>
  <text x="25" y="206" font-size="9" font-weight="bold" fill="#991b1b">3. 3D real con iluminación</text>
  <text x="25" y="222" font-size="7" fill="#475569">SVG es vectorial 2D. Simular 3D con</text>
  <text x="25" y="236" font-size="7" fill="#475569">proyección es posible pero frágil</text>
  <text x="25" y="250" font-size="7" fill="#475569">Three.js o WebGPU son el camino para</text>
  <text x="25" y="264" font-size="7" fill="#991b1b">  escenas 3D con luz, sombras, materiales</text>
</svg>
```

---

## Qué librería para qué tipo de trabajo

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    elegir bien ahorra semanas
  </text>

  <rect x="10" y="32" width="280" height="20" fill="#e2e8f0"/>
  <text x="20" y="46" font-size="7" font-weight="bold" fill="#1e293b">Necesito…</text>
  <text x="170" y="46" font-size="7" font-weight="bold" fill="#1e293b">Librería</text>

  <rect x="10" y="52" width="280" height="22" fill="white"/>
  <text x="20" y="66" font-size="7" fill="#475569">Dashboards con datos cambiantes</text>
  <text x="170" y="66" font-size="7" fill="#3b82f6">D3.js</text>

  <rect x="10" y="74" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="88" font-size="7" fill="#475569">Timelines y secuencias complejas</text>
  <text x="170" y="88" font-size="7" fill="#3b82f6">GSAP</text>

  <rect x="10" y="96" width="280" height="22" fill="white"/>
  <text x="20" y="110" font-size="7" fill="#475569">Animaciones simples, ligero</text>
  <text x="170" y="110" font-size="7" fill="#3b82f6">Anime.js o CSS</text>

  <rect x="10" y="118" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="132" font-size="7" fill="#475569">Line-drawing del logo</text>
  <text x="170" y="132" font-size="7" fill="#3b82f6">Vivus.js</text>

  <rect x="10" y="140" width="280" height="22" fill="white"/>
  <text x="20" y="154" font-size="7" fill="#475569">Onboarding de After Effects</text>
  <text x="170" y="154" font-size="7" fill="#3b82f6">Lottie</text>

  <rect x="10" y="162" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="176" font-size="7" fill="#475569">Sistema de iconos React</text>
  <text x="170" y="176" font-size="7" fill="#3b82f6">SVGR (solo build)</text>

  <rect x="10" y="184" width="280" height="22" fill="white"/>
  <text x="20" y="198" font-size="7" fill="#475569">Crear SVG con API fluida</text>
  <text x="170" y="198" font-size="7" fill="#3b82f6">SVG.js</text>

  <rect x="10" y="206" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="220" font-size="7" fill="#475569">App de dibujo interactiva</text>
  <text x="170" y="220" font-size="7" fill="#3b82f6">Paper.js (Canvas)</text>

  <rect x="15" y="238" width="270" height="34" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="254" font-size="9" font-weight="bold" fill="#166534">Antes de importar</text>
  <text x="25" y="266" font-size="7" fill="#475569">pregunta: ¿resolverlo a mano costaría más?</text>
</svg>
```

---

## Qué aprender después

```svg
<svg width="300" height="300"
     viewBox="0 0 300 300"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    3 direcciones para ir más lejos
  </text>

  <rect x="15" y="38" width="270" height="80" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#1e40af">1. Profundidad en SVG</text>
  <text x="25" y="70" font-size="7" fill="#475569">• filtros de iluminación (feDiffuseLighting)</text>
  <text x="25" y="84" font-size="7" fill="#475569">• morphing avanzado con GSAP MorphSVG</text>
  <text x="25" y="98" font-size="7" fill="#475569">• SVG en Web Components + Shadow DOM</text>
  <text x="25" y="112" font-size="7" fill="#1e40af">• especificación SVG 2 completa</text>

  <rect x="15" y="126" width="270" height="80" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="142" font-size="9" font-weight="bold" fill="#166534">2. Ecosistema de datos</text>
  <text x="25" y="158" font-size="7" fill="#475569">• D3.js a fondo: escalas, layouts, transiciones</text>
  <text x="25" y="172" font-size="7" fill="#475569">• Observable Plot: alto nivel sobre D3</text>
  <text x="25" y="186" font-size="7" fill="#475569">• Visx, Recharts: wrappers React</text>
  <text x="25" y="200" font-size="7" fill="#166534">• ECharts, Highcharts si prefieres Canvas</text>

  <rect x="15" y="214" width="270" height="80" fill="#ede9fe" stroke="#8b5cf6" rx="3"/>
  <text x="25" y="230" font-size="9" font-weight="bold" fill="#5b21b6">3. Más allá de SVG</text>
  <text x="25" y="246" font-size="7" fill="#475569">• Canvas API para graphics de alto volumen</text>
  <text x="25" y="260" font-size="7" fill="#475569">• Three.js / WebGPU para 3D real</text>
  <text x="25" y="274" font-size="7" fill="#475569">• Shaders GLSL para efectos custom</text>
  <text x="25" y="288" font-size="7" fill="#5b21b6">• Generative art (p5.js, paper.js, processing)</text>
</svg>
```

---

## La última regla: simplicidad primero

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    la regla que sobrevive a todo
  </text>

  <rect x="15" y="40" width="270" height="60" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="58" font-size="10" font-weight="bold" fill="#166534">HTML/CSS antes que SVG</text>
  <text x="25" y="74" font-size="7" fill="#475569">si un botón redondo se puede hacer con</text>
  <text x="25" y="88" font-size="7" fill="#475569">border-radius, no dibujes un &lt;circle&gt;</text>

  <rect x="15" y="108" width="270" height="60" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="126" font-size="10" font-weight="bold" fill="#1e40af">SVG antes que librerías</text>
  <text x="25" y="142" font-size="7" fill="#475569">si una animación se puede hacer con 3 líneas</text>
  <text x="25" y="156" font-size="7" fill="#475569">de CSS, no importes GSAP ni Anime.js</text>

  <rect x="15" y="176" width="270" height="52" fill="#ede9fe" stroke="#8b5cf6" rx="3"/>
  <text x="25" y="192" font-size="10" font-weight="bold" fill="#5b21b6">Librerías antes que reescribir</text>
  <text x="25" y="208" font-size="7" fill="#475569">si D3/GSAP/Lottie ya resuelven tu caso,</text>
  <text x="25" y="220" font-size="7" fill="#475569">no reinventes la rueda en 2.000 líneas de JS</text>
</svg>
```
