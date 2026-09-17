# fixcomap/web

Landing de [fixcomap.com](https://fixcomap.com) y `www`. HTML/CSS estático, sin framework ni build.
Servido por Cloudflare Pages. El pipeline no vive aquí: `ci.yml` llama a los *reusable workflows* de
`fixcomap/platform` (`rw-static-checks`, `rw-pages-deploy`). Cada PR publica una preview
`<rama>.fixcomap-landing.pages.dev`, cada push a `develop` la preview `develop.…` y cada push a `main`
producción (environment `production`). El token de Cloudflare está acotado a Pages y es distinto del de DNS.

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
| `package.json` | `stylelint` (CI) y `wrangler` (rollback en local); nunca se publica (está fuera de `public/`) |

## Flujo

GitFlow como en `platform`: `feature/*` → `develop` (PR, 0 aprobaciones, checks en verde) →
`release/*` o `hotfix/*` → `main` (PR, 1 aprobación). Sin push directo a ninguna de las dos.

Checks en PR (`rw-static-checks` de `platform`): rama origen válida, `gitleaks`, `trivy` (secretos y
misconfig), validación HTML (W3C Nu), CSS (`stylelint`), y que `_headers` siga con CSP. Los pines de
Actions y de `wrangler` los mantiene Renovate en `platform`.

## Contacto

`contacto@fixcomap.com` (Cloudflare Email Routing).
