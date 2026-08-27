# ZVIEW.AIR

Sitio web de fine art prints de fotografía aérea (drone) de Felipe Sepúlveda, bajo la marca **ZVIEW.AIR**. Landing de una sola marca con catálogo de obras, formatos/precios, proceso de compra y formulario de encargo personalizado (con contacto directo por WhatsApp).

- **Sitio en producción:** ver Instagram [@zview.air](https://instagram.com/zview.air)
- **Repo:** [github.com/fasep/zviewair](https://github.com/fasep/zviewair)

## Stack

Sitio estático puro, sin build ni dependencias de paquetes:

- HTML + CSS (inline en cada página) + JavaScript vanilla
- Tipografías: Google Fonts (`Cormorant Garamond`, `Space Mono`)
- Sin framework, sin backend — el formulario de contacto es un mock (no envía datos a ningún servidor)

## Estructura

```
index.html          Landing principal (hero, colección, formatos, proceso, sobre mí, encargo)
detalle.html         Página de detalle de una obra, recibe ?id=<slug> por query string
favicon.svg
images/              Fotografías de las obras
images/mockups/      Mockups de las obras enmarcadas/en ambientes, usados en detalle.html
```

No hay proceso de build: se edita el HTML directamente y se sirve tal cual.

## Cómo correrlo localmente

Al ser estático, alcanza con un servidor HTTP simple desde la carpeta del proyecto:

```bash
python3 -m http.server 8000
# abrir http://localhost:8000
```

(Abrir `index.html` con doble click también funciona, salvo por rutas relativas en algunos casos de fetch/CORS que no aplican acá.)

## Cómo agregar una obra nueva

1. Agregar la imagen en `images/` (y sus mockups, si hay, en `images/mockups/`).
2. En `index.html`, dentro de `.products-grid` (sección `#coleccion`), copiar un bloque `.product-card` existente y ajustar:
   - `data-product`: slug único (ej. `mi-obra-nueva`)
   - `src` de la imagen
   - `product-name`, `product-size`
   - el link `detalle.html?id=<slug>`
3. En `detalle.html`, dentro del objeto `products` (línea ~304), agregar una entrada con el mismo slug:
   ```js
   'mi-obra-nueva': {
     title: 'Mi<br><em>Obra Nueva</em>',
     image: 'images/mi_obra.jpg',
     mockups: ['images/mockups/mi_obra_sala.jpg']   // opcional, puede ir []
   }
   ```

`detalle.html` lee el slug desde la URL (`?id=...`), busca la entrada en `products` y renderiza el hero y la grilla de mockups. Si el slug no existe, redirige a `index.html`.

## Contacto / integración externa

- **WhatsApp:** botón flotante y CTA del formulario apuntan a `https://wa.me/56988078398`. El número está hardcodeado en ambos HTML — buscar `wa.me` para actualizarlo.
- **Instagram:** `@zview.air`, linkeado en el nav/stats y footer.
- **Formulario de encargo** (`#encargo` en `index.html`): es un mock — `handleSubmit()` sólo cambia el texto del botón, no envía el formulario a ningún lado. Si se quiere que efectivamente llegue el mensaje, hay que conectarlo a un backend/servicio de formularios (ej. Formspree, un Worker propio, etc.).

## Deploy

El repo es un sitio estático simple, por lo que se puede publicar directo en GitHub Pages, Netlify, Vercel o Cloudflare Pages sin configuración de build (root = raíz del repo, sin build command).
