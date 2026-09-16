# fixcomap/web

Landing de [fixcomap.com](https://fixcomap.com) y `www`. HTML/CSS estático, sin framework ni build.
Servido por Cloudflare Pages, que hace pull de este repo: cada push a `main` publica en producción y
cada push a `develop` genera una preview en `*.fixcomap-landing.pages.dev`. Sin credenciales en Actions.

El proyecto de Pages, sus dominios y el DNS viven como código en
[`fixcomap/platform`](https://github.com/fixcomap/platform) (`infra/dns/pages.tf`).

## Estructura

| Fichero | Qué es |
|---|---|
| `public/` | Lo que Pages publica, tal cual (output directory) |
| `public/index.html`, `public/style.css` | La página |
| `public/_headers` | Cabeceras que aplica Pages (CSP estricta: sin JS) |
| `public/robots.txt` | Indexable |
| `public/assets/` | Fotos y CVs; nombres exactos en `public/assets/README.md` |
| `package.json` | Solo `stylelint` para CI; nunca se publica (está fuera de `public/`) |

## Flujo

GitFlow como en `platform`: `feature/*` → `develop` (PR, 0 aprobaciones, checks en verde) →
`release/*` o `hotfix/*` → `main` (PR, 1 aprobación). Sin push directo a ninguna de las dos.

Checks en PR (`pr-checks.yml`): rama origen válida, `gitleaks`, `trivy` (secretos y misconfig),
validación HTML (W3C Nu), CSS (`stylelint`), y que `_headers` siga con CSP. Actions pineadas por SHA;
Renovate las mantiene.

## Contacto

`hola@fixcomap.com` (Cloudflare Email Routing).
