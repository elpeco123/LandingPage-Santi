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

### Ojo con el Referrer-Policy

`netlify.toml` fija `Referrer-Policy = "strict-origin-when-cross-origin"` a propósito.
YouTube valida el referrer para autorizar el embed: si alguien lo cambia a
`no-referrer`, el reproductor deja de andar en producción con el error 153.

## Después del deploy

Con el dominio final ya asignado, agregá la URL canónica en el `<head>` de
`index.html` para que la vista previa al compartir quede completa:

```html
<meta property="og:url" content="https://TU-DOMINIO.netlify.app/">
<link rel="canonical" href="https://TU-DOMINIO.netlify.app/">
```

## Qué hay que mantener

Los beats están escritos a mano en el `DIR.LISTING` de `index.html`. Para sumar uno
nuevo de la serie, copiá un bloque `<button class="row">` y cambiá `data-yt` (el ID
del video de YouTube), `data-name` y el número. Acordate de actualizar el conteo
del header del módulo (`3 ARCHIVOS`).
