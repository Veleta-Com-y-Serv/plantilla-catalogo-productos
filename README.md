# Plantilla NoExpertise · Catálogo de productos

Web técnica para mostrar productos con referencias, características y botón de pedir presupuesto.

- Sectores: maquinaria, industria, mayorista, suministros, catalogo, ferreteria. Estilo: técnico. Plan mínimo: presencia.
- Secciones: hero, categorias, productos, ventajas, proceso, clientes, preguntas, contacto (+ `texto` genérico).
- Campos propios: `productos[]` (nombre, ref, categoria, texto, caracteristicas, precio, estado, imagen), `categorias.items[].imagen/url`, `clientes.items[]`.
- SEO de base incluido (`src/lib/seo.ts`): canónica, og:image, JSON-LD del negocio y de las preguntas frecuentes, `robots.txt`, `sitemap.xml` y `llms.txt`; `noindex` si `seo.indexable: false`.
- Contenido editable en `site.yaml`; metadatos en `template.yaml`; reglas para el agente en `CLAUDE.md`; fotos a generar en `fotos.json`.

```
npm ci
npm run dev      # desarrollo
npm run build    # web estática en dist/
```

Basada en [AstroWind](https://github.com/onwidget/astrowind) © onWidget, licencia MIT (`LICENSE-ASTROWIND.md`). Imágenes de relleno (`public/placeholders/`) dibujadas para esta plantilla, sin licencias de terceros.
