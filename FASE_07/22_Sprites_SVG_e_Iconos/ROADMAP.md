# Seccion 22: Sprites SVG e Iconos

> Un sistema de iconos SVG bien construido es reutilizable, tematizable y accesible.
> La tecnica de `<symbol>` + `<use>` es el estandar actual.
> Cada decision de arquitectura (inline, externo, componentes) tiene consecuencias reales.

---

## 22.1 Sistema de iconos con SVG: por que SVG gana

### Frente a icon fonts (FontAwesome, Material Icons como fuente)

| Aspecto | Icon fonts | SVG |
|---------|-----------|-----|
| Colores multiples | No (un solo color CSS) | Si (cada parte independiente) |
| Precision visual | Depende del render de fuentes | Siempre exacto (vectorial) |
| Posicionamiento | Tricky (baseline, line-height) | Control total |
| Accesibilidad | Problematico (caracteres especiales) | Nativo con title/aria |
| Subconjunto | Build tools necesarios | Facil (solo incluir los usados) |
| Fallos de carga | Cuadros vacios o simbolos raros | Fallback controlable |
| Animacion | No | Si |

### Frente a imagenes raster (PNG, WebP)

| Aspecto | PNG/WebP | SVG |
|---------|----------|-----|
| Calidad en Retina | Requiere @2x, @3x | Una version para todo |
| Cambio de color | Imposible en CSS | `currentColor`, variables |
| Tamano de archivo | Variable (puede ser grande) | Generalmente menor para iconos |
| Animacion | No | Si |
| Interactividad | No | Si |

### Conclusion
SVG es el formato correcto para sistemas de iconos en aplicaciones web modernas.
La unica excepcion: iconos muy complejos (fotograficos) donde un PNG puede ser mas eficiente.

---

## 22.2 SVG Sprite con `<symbol>` y `<use>`

### La arquitectura del sprite

Un **sprite SVG** es un unico archivo (o bloque SVG) que contiene todos los iconos como `<symbol>`:

```xml
<svg xmlns="http://www.w3.org/2000/svg" style="display:none">
  <symbol id="icon-home" viewBox="0 0 24 24">
    <!-- path del icono de home -->
    <path d="M10 20v-6h4v6h5v-8h3L12 3 2 12h3v8z"/>
  </symbol>

  <symbol id="icon-user" viewBox="0 0 24 24">
    <!-- path del icono de usuario -->
    <path d="M12 12c2.21 0 4-1.79 4-4s-1.79-4-4-4-4 1.79-4 4 1.79 4 4 4z"/>
  </symbol>

  <symbol id="icon-close" viewBox="0 0 24 24">
    <path d="M19 6.41L17.59 5 12 10.59 6.41 5 5 6.41 10.59 12 5 17.59 6.41 19 12 13.41 17.59 19 19 17.59 13.41 12z"/>
  </symbol>
</svg>
```

### Por que `<symbol>` y no `<g>`

- `<symbol>` tiene su propio `viewBox`: el icono se define en su propio sistema de coordenadas
- Al usar `<use width="24" height="24">`, el icono se escala correctamente al tamano pedido
- Con `<g>`, el icono viviria en el sistema de coordenadas del SVG padre y el escalado seria mas complejo
- `<symbol>` nunca se renderiza directamente: perfecto para una biblioteca de iconos

### Usar el icono con `<use>`

```html
<!-- En cualquier parte del HTML -->
<svg width="24" height="24">
  <use href="#icon-home" />
</svg>

<!-- Con clase para control CSS -->
<svg class="icono" aria-hidden="true" focusable="false">
  <use href="#icon-home" />
</svg>
```

```css
.icono {
  width: 1.5rem;
  height: 1.5rem;
  fill: currentColor;   /* hereda el color del texto del contexto */
}
```

### `display: none` vs `hidden` en el sprite contenedor

Para ocultar el SVG contenedor del sprite sin que ocupe espacio:

**`style="display:none"`** — recomendado
- No ocupa espacio en el layout
- Los `<symbol>` siguen siendo referenciables con `<use>`
- Funciona en todos los navegadores

**Atributo `hidden`**
- HTML nativo para ocultar elementos
- Equivalente a `display: none` en la mayoria de contextos
- Algunos lectores de pantalla antiguos pueden tener comportamientos inesperados con SVG oculto asi

**`width="0" height="0" style="position:absolute"`** (tecnica antigua)
- Se usaba antes de que `display:none` funcionara bien con sprites SVG
- Ya no es necesaria en navegadores modernos

### Accesibilidad del sprite y de los iconos

**El sprite contenedor**
```html
<!-- El sprite no necesita ser accesible por si mismo -->
<svg aria-hidden="true" focusable="false" style="display:none">
  <symbol id="icon-home" viewBox="0 0 24 24">...</symbol>
</svg>
```

**El uso del icono: dos casos**

*Icono decorativo (acompanado de texto)*
```html
<button>
  <svg aria-hidden="true" focusable="false" class="icono">
    <use href="#icon-home" />
  </svg>
  Inicio
</button>
```

*Icono con significado (sin texto)*
```html
<button aria-label="Volver al inicio">
  <svg aria-hidden="true" focusable="false" class="icono">
    <use href="#icon-home" />
  </svg>
</button>
```

---

## 22.3 SVG Sprite externo

### Como funciona

El sprite se guarda como un archivo `.svg` separado y los iconos se referencian con una URL:

```html
<svg class="icono">
  <use href="/assets/icons/sprite.svg#icon-home" />
</svg>
```

### Ventajas del sprite externo

**Cacheable independientemente**
- El archivo `sprite.svg` se cachea separadamente del HTML
- Si se usa en multiples paginas, se descarga una sola vez
- Actualizaciones del sprite no invalidan la cache del HTML

**HTML mas limpio**
- No hay bloque SVG grande en el HTML
- El HTML es mas legible y mantenible

**Un solo archivo para toda la aplicacion**
- Todos los iconos en un lugar centralizado
- Facil de actualizar y mantener

### Limitaciones del sprite externo

**Restricciones de CORS**
- El archivo SVG externo debe estar en el **mismo origen** (dominio + puerto + protocolo)
- Un sprite en `https://cdn.ejemplo.com/sprite.svg` no puede usarse desde `https://ejemplo.com` sin CORS
- Configurar `Access-Control-Allow-Origin` en el servidor del sprite si es cross-origin

**No se puede estilizar con CSS externo**
- El CSS del documento padre no puede penetrar en el shadow DOM de `<use>`
- Solo `currentColor` y variables CSS heredadas funcionan para el estilo

**Soporte en IE/Edge legacy**
- IE y Edge antiguo no soportaban referencias externas con `<use href="sprite.svg#id">`
- Se necesitaba el polyfill `svg4everybody` o `svgxuse`
- En proyectos modernos sin soporte IE: no es un problema

### Servir el sprite correctamente

El servidor debe enviar el SVG con el Content-Type correcto:
```
Content-Type: image/svg+xml
```

Y con los headers de cache apropiados para aprovechar el caching del navegador:
```
Cache-Control: public, max-age=31536000, immutable
```

Si el nombre del archivo incluye un hash de contenido (cache busting), usar `immutable`.

---

## 22.4 SVG inline como componentes (React, Vue, etc.)

### El patron de componente SVG

En frameworks modernos, cada icono es un componente que retorna SVG inline:

**React**
```jsx
// HomeIcon.jsx
export function HomeIcon({ size = 24, color = 'currentColor', ...props }) {
  return (
    <svg
      width={size}
      height={size}
      viewBox="0 0 24 24"
      fill={color}
      aria-hidden="true"
      focusable="false"
      {...props}
    >
      <path d="M10 20v-6h4v6h5v-8h3L12 3 2 12h3v8z" />
    </svg>
  );
}

// Uso
<HomeIcon size={32} color="steelblue" />
<HomeIcon className="mi-icono" aria-label="Inicio" />
```

**Vue**
```vue
<!-- HomeIcon.vue -->
<template>
  <svg :width="size" :height="size" viewBox="0 0 24 24" :fill="color">
    <path d="M10 20v-6h4v6h5v-8h3L12 3 2 12h3v8z" />
  </svg>
</template>

<script>
export default {
  props: {
    size: { type: Number, default: 24 },
    color: { type: String, default: 'currentColor' }
  }
}
</script>
```

### Ventajas del enfoque de componentes

**Maximo control**
- Props para color, tamano, variante
- Logica condicional: mostrar una version del icono u otra segun el estado
- Eventos directamente en el componente
- Accesibilidad integrada en el componente

**Herramientas de desarrollo**
- Autocompletado de nombres de icono
- Tree-shaking: solo se incluyen en el bundle los iconos que se usan
- Facil de encontrar que componentes usan un icono especifico (via busqueda en el codigo)

### Herramientas para generar componentes SVG

**SVGR** (React)
- Convierte archivos `.svg` a componentes React automaticamente
- CLI: `npx @svgr/cli --out-dir src/icons src/assets/icons`
- Plugin para Vite: `@svgr/vite`
- Plugin para webpack: `@svgr/webpack`

**vue-svg-loader** (Vue)
- Importar SVG como componentes Vue directamente
- `import HomeIcon from './home.svg'`

**Astro SVG**
- Soporte nativo de SVG en componentes Astro

---

## 22.5 Organizacion de un sistema de iconos

### Convencion de naming para IDs

Adoptar una convencion consistente desde el inicio:

**Prefijo + nombre semantico**
- `icon-home`, `icon-user`, `icon-close` — simple y claro
- `ic-arrow-right`, `ic-arrow-left` — prefijo corto
- `i-chevron-down` — muy corto

**Variantes del mismo icono**
- `icon-arrow-right`, `icon-arrow-left`, `icon-arrow-up`, `icon-arrow-down`
- `icon-heart`, `icon-heart-filled`, `icon-heart-outline`
- `icon-alert-info`, `icon-alert-warning`, `icon-alert-error`

**Evitar**
- IDs con espacios: `id="my icon"` — invalido
- IDs que empiezan con numeros: `id="24px-home"` — invalido en HTML4
- IDs con caracteres especiales: guiones (`-`) y guiones bajos (`_`) son seguros

### Tamano base y viewBox estandar

**Estandares comunes**

| Tamano | Uso tipico |
|--------|-----------|
| `viewBox="0 0 16 16"` | Iconos muy pequenios, inline en texto |
| `viewBox="0 0 24 24"` | **El mas comun** en sistemas UI (Material, Heroicons, etc.) |
| `viewBox="0 0 32 32"` | Iconos de tamano medio |
| `viewBox="0 0 48 48"` | Iconos grandes, ilustrativos |

**Recomendacion**: elegir un tamano base y ser consistente. `0 0 24 24` es el estandar de facto.

### El icono en diferentes tamanos

Con el viewBox estandar, el icono puede renderizarse en cualquier tamano desde CSS:
```css
/* Sistema de tamanos tipico */
.icon-sm  { width: 16px; height: 16px; }
.icon-md  { width: 24px; height: 24px; }
.icon-lg  { width: 32px; height: 32px; }
.icon-xl  { width: 48px; height: 48px; }
```

### `currentColor` como fill por defecto

El patron para iconos que heredan el color del contexto:

```xml
<symbol id="icon-home" viewBox="0 0 24 24">
  <path fill="currentColor" d="..." />
</symbol>
```

O en el nivel del `<use>`:
```css
svg { fill: currentColor; }
```

**Ventaja**: el icono automaticamente toma el color del texto del boton, enlace o elemento padre.

### Documentacion del sistema de iconos

Un sistema de iconos bien organizado incluye:
- **Catalogo visual**: lista de todos los iconos con su nombre e ID
- **Guia de uso**: como importar, como cambiar el color y el tamano
- **Politica de contribucion**: como agregar nuevos iconos (tamano, estilo, convencion de nombre)
- **Historial de cambios**: que iconos se agregaron, renombraron o eliminaron

Herramientas como Storybook pueden usarse para documentar y visualizar el sistema de iconos.
