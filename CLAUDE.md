# Plantilla «Catálogo de productos» (NoExpertise)

Web estática Astro 5 + Tailwind 4, derivada de AstroWind (MIT, ver `LICENSE-ASTROWIND.md`). Estilo: **técnico**
(gris acero + naranja de seguridad, sans del sistema, referencias en monoespaciada, esquinas de 2 px, bordes y tablas).
Todo en castellano de España, frases cortas, sin jerga.

## Dónde está cada cosa
- **Textos, contacto, menú, SEO y secciones**: `site.yaml` (raíz). Es la única fuente de contenido. `src/data/site.ts` lo carga y lo tipa; no pongas textos fijos en los componentes.
- **Colores**: `site.yaml → colores` (primario = acero, acento = naranja, fondo, texto). `src/layouts/Base.astro` los convierte en variables CSS (`--c-primario`…) y Tailwind los expone como `bg-primario`, `text-acento`, `bg-primario/10`… Los derivados (`--c-acento-texto`, `--c-oscuro`, `--c-borde`, `--c-panel`, `--c-rejilla`) se calculan con `color-mix` en `src/styles/global.css`. El texto sobre naranja es oscuro (`--c-sobre-acento`): si el cliente pone un acento oscuro, cambia esa variable.
- **Tipografías y aspecto general**: `src/styles/global.css` (`--fuente-mono` para referencias; botones `.btn` naranja, `.btn-oscuro` acero, `.btn-sec` contorno; `.panel`, `.rejilla-tecnica`, `.tabla-specs`, `.estado`, `.filtro`; cabecera `.utilidades` + `.cabecera--tecnica` + `.categorias-barra`; pie `.pie--tecnico`). Fuentes del sistema: no añadas fuentes externas.
- **Cabecera** (`src/components/Header.astro`): barra de utilidades (teléfono, email, horario), marca + menú + `boton_cabecera`, y barra de categorías con los títulos de `categorias.items` (al pulsar, filtra el catálogo).
- **Pie** (`src/components/Footer.astro`): columnas de categorías, empresa (`menu`) y contacto.
- **Imágenes**: en `public/`. Fotos en `public/fotos/` (las genera el coordinador desde `fotos.json`); respaldo en `public/placeholders/`. En `site.yaml` se referencian con ruta absoluta (`/fotos/producto-1.jpg`). Si una foto falta, `Imagen.astro` muestra su SVG de respaldo. Fotos de producto sobre fondo blanco o neutro (se integran con la rejilla).
- **Logo**: `site.yaml → logo` (ruta en `public/`); vacío = inicial + nombre en texto.
- **Pie «Web creada con NoExpertise»**: `site.yaml → marca_pie` (se oculta solo con `false`).

## Secciones
Cada sección es un componente que solo se pinta si `site.yaml → secciones.<id>.activa` es `true`. Se pintan en el orden en que aparecen en `site.yaml`.
- `hero` → `secciones/hero.astro` · texto + botones a la izquierda, foto de producto sobre rejilla técnica a la derecha.
- `categorias` → `secciones/categorias.astro` · mosaico de familias (con foto o icono).
- `productos` → `secciones/productos.astro` · fichas con referencia, tabla de características, precio y «Consultar»; filtro por categoría.
- `ventajas` → `secciones/ventajas.astro` · banda acero con casillas e iconos.
- `proceso` → `secciones/proceso.astro` · pasos numerados.
- `clientes` → `secciones/clientes.astro` · logos en gris o nombres (no se pinta si la lista está vacía).
- `preguntas` → `secciones/preguntas.astro` · acordeón `details/summary` (no se pinta si está vacía).
- `contacto` → `secciones/contacto.astro` · datos + formulario con «Producto de interés».
- `texto` → `secciones/texto.astro` · apartado genérico para secciones nuevas (`tipo: texto`).

## Campos propios de esta plantilla (además del esquema común)
- `hero.items[]`: datos breves bajo los botones (`titulo` = dato, `texto` = etiqueta). Vacío = no se pinta. No inventes cifras.
- `categorias.items[]`: `{ titulo, texto, icono, imagen, url? }`. Con `imagen` se muestra la foto; sin ella, el `icono`. El `titulo` debe coincidir con `productos[].categoria` para que el filtro funcione. `url` opcional sustituye al filtro.
- `productos.productos[]`: `{ nombre, ref, categoria, texto, caracteristicas, precio, estado, imagen }`.
  - `caracteristicas`: **una cadena** con pares separados por «;» → tabla (`"Potencia: 7,5 kW; Caudal: 1.050 l/min"`). Sin «:», la parte ocupa la fila entera. Nunca una lista.
  - `precio`: **cadena**. `"Consultar"` o vacío = no se muestra precio (solo el botón). Nunca inventes precios.
  - `estado`: `"Nuevo"`, `"Ocasión"` o `""` (vacío = sin etiqueta).
  - `ref`: referencia, se muestra en monoespaciada. Vacía = no sale.
  - Lista vacía (`productos: []`) = se muestra el `texto` y el botón `cta` en un recuadro, sin rejilla.
  - El botón «Consultar» lleva a la sección `contacto` y deja el producto elegido en el formulario y en el asunto.
- `productos.cta`: botón bajo la rejilla (p. ej. «Pedir el catálogo completo»).
- `ventajas.items[]` y `proceso.items[]`: `{ titulo, texto, icono? }`.
- `clientes.items[]`: `{ titulo, imagen? }` (nombre del cliente y logo opcional en gris). No inventes clientes.
- `preguntas.preguntas[]`: `{ pregunta, respuesta }`.
- Iconos disponibles: ver `src/components/ui/Icono.astro` (compresor, rayo, llama, elevacion, gota, herramienta, caja, camion, engranaje, enchufe, tarjeta, escudo…).

## Reglas para órdenes
1. **Activar o quitar una sección**: cambia `activa: true|false` en `site.yaml`. No borres componentes.
2. **Reordenar secciones**: mueve el bloque dentro de `secciones:` en `site.yaml`.
3. **Cambiar textos, teléfonos, horarios, fotos, productos**: solo en `site.yaml`. Si un texto contiene comas dentro de `{ … }` (también «7,5 kW»), ponlo entre comillas.
4. **Listas planas**: ningún elemento de lista puede llevar otra lista dentro. Máximo 24 elementos por lista.
5. **Página nueva**: crea `src/pages/<slug>.astro` usando `import Base from '../layouts/Base.astro'` y `<Base titulo="…" descripcion="…">…</Base>`; enlázala en `site.yaml → menu`. Ejemplo: `src/pages/aviso-legal.astro`.
6. **Sección nueva**: mejor con `tipo:` (`tipo: texto`, `tipo: ventajas`, `tipo: productos`…) y un id propio en `site.yaml`. Si hace falta un diseño nuevo, crea `src/components/secciones/<id>.astro` copiando la estructura de una existente (`seccion(Astro.props.id)`, `Seccion`, `Titular`). `src/pages/index.astro` la recoge sola.
7. **No tocar** `src/layouts/Base.astro`, `src/pages/index.astro`, `tailwind.config.mjs` ni `src/styles/comun.css` salvo orden explícita de diseño. Los ids `hero` y `contacto` no se cambian. Tampoco `src/lib/seo.ts` ni `src/pages/robots.txt.ts`, `sitemap.xml.ts` y `llms.txt.ts`: son el SEO de base (canónica, og:image, `noindex` si `seo.indexable: false`, JSON-LD del negocio y de las preguntas, robots, sitemap y llms.txt), salen solos de `site.yaml` y el fichero es el mismo en las 16 plantillas. `seo.url` y `seo.indexable` los pone la plataforma: no los cambies.
8. Mantén: `lang="es-ES"`, imágenes con `Imagen.astro` (carga diferida), sin JavaScript nuevo salvo necesidad real (ya hay tres scripts mínimos: menú móvil, filtro del catálogo y asunto del formulario), 16 px de margen lateral (`.contenedor`), sin scroll horizontal en móvil. Imágenes de contenido con `alt` sacado del contenido (`altHero`, `altFicha`, `altGaleria`, `altSeccion` de `src/lib/seo.ts`, o el `titulo`/`nombre` de la ficha); las decorativas, `alt=""` (Imagen.astro les pone `aria-hidden`).
9. El formulario no tiene servidor: abre el correo (`mailto`) con el email de `site.yaml`. Para envío real, cambia `action` en `src/components/ui/FormularioContacto.astro`.

## Construir
```
npm ci && npm run build      # resultado en dist/
npx astro preview            # ver dist/ en http://localhost:4321
```
Antes de dar una orden por buena: `npm run build` sin errores.
