# Finn Capitel — sitio web

Sitio estático (`index.html` + `assets/`), sin dependencias ni paso de compilación.
Se publica en **Cloudflare Workers & Pages** desde este repositorio: cada `git push` a `main` genera un despliegue nuevo.

## Ver el sitio en local

```bash
python3 -m http.server 8000
```

Después abre http://localhost:8000.

## Publicar en Cloudflare (solo la primera vez)

1. Entra a [dash.cloudflare.com](https://dash.cloudflare.com) → **Compute (Workers)** → **Workers & Pages** → **Create application**.
2. Elige **Import a repository** / **Connect to Git** y autoriza a Cloudflare en GitHub para acceder a `christianmendozacruz-web/FinnCapitel`.
3. Selecciona el repositorio y usa esta configuración:

   | Campo | Valor |
   | --- | --- |
   | Project name | `finn-capitel` |
   | Production branch | `main` |
   | Build command | *(vacío)* |
   | Deploy command | `npx wrangler deploy` |
   | Root directory | `/` |

4. Pulsa **Deploy**. El sitio queda en `https://finn-capitel.<tu-subdominio>.workers.dev`.

La configuración del Worker está en `wrangler.jsonc`. El archivo `.assetsignore` evita que se publiquen archivos que no son del sitio, como este README.

## Actualizar el sitio

```bash
git add -A
git commit -m "Describe el cambio"
git push
```

Cloudflare despliega solo. El avance se ve en **Workers & Pages → finn-capitel → Deployments**.

## Dominio

El sitio responde en `finncapitel.com` y `www.finncapitel.com` mediante las rutas declaradas en `wrangler.jsonc`. Los registros A de la raíz siguen en el DNS, pero el Worker atiende todo el tráfico web, así que no se usan.

Las rutas no afectan al correo de Microsoft 365 (MX, SPF, DMARC, autodiscover). No borres esos registros.

## Volver a una versión anterior

En **Deployments**, abre una versión anterior y usa **Rollback**.
