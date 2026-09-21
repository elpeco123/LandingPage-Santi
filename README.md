# prod by rio — VOID/FOLDER

Landing page de **prod by rio**. Una sola página estática, sin build ni dependencias:
todo el sitio es `index.html`.

## Ver el sitio localmente

El reproductor de YouTube **no funciona** si abrís `index.html` con doble click.
Los embeds necesitan un origen `http(s)://`; con `file://` YouTube devuelve el
**error 153**. Levantá un servidor:

```bash
python -m http.server 8777
```

Y entrá a <http://localhost:8777>.

## Subir a Netlify

No hay paso de build. `netlify.toml` ya deja configurado el directorio de
publicación y las cabeceras.

1. En Netlify: **Add new site → Import an existing project → GitHub**.
2. Elegí este repositorio.
3. Dejá **Build command** vacío y **Publish directory** en `.` (Netlify lo lee de `netlify.toml`).
4. Deploy.
5. En **Site configuration → Change site name**, poné `prodrio`.

El paso 5 no es opcional: `index.html` ya declara `https://prodrio.netlify.app/`
como URL canónica. Si el sitio queda con el nombre aleatorio que asigna Netlify,
la vista previa al compartir el link va a apuntar a un dominio que no existe.

### Ojo con el Referrer-Policy

`netlify.toml` fija `Referrer-Policy = "strict-origin-when-cross-origin"` a propósito.
YouTube valida el referrer para autorizar el embed: si alguien lo cambia a
`no-referrer`, el reproductor deja de andar en producción con el error 153.

## Si cambiás de dominio

La URL está escrita en dos lugares del `<head>` de `index.html`. Si algún día
conectás un dominio propio, actualizá los dos:

```html
<link rel="canonical" href="https://prodrio.netlify.app/">
<meta property="og:url" content="https://prodrio.netlify.app/">
```

## Qué hay que mantener

Los beats están escritos a mano en el `DIR.LISTING` de `index.html`. Para sumar uno
nuevo de la serie, copiá un bloque `<button class="row">` y cambiá `data-yt` (el ID
del video de YouTube), `data-name` y el número. Acordate de actualizar el conteo
del header del módulo (`3 ARCHIVOS`).
