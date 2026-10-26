# fixcomap/web

Landing de [fixcomap.com](https://fixcomap.com) y `www`. HTML/CSS estático, sin framework ni build.

## Cómo se despliega

Este repo no tiene pipeline propio. `.github/workflows/ci.yml` es el mismo en todos los repos de
fixcomap y llama al pipeline estándar de [`fixcomap/platform`](https://github.com/fixcomap/platform),
que lee [`catalog/web.yaml`](https://github.com/fixcomap/platform/blob/main/catalog/web.yaml)
para saber qué es este repo (`type: static`) y cómo publicarlo. El proyecto de Cloudflare Pages
(`fixcomap-landing`), los dominios y el DNS también salen de ese fichero.

| Evento | Qué pasa |
|---|---|
| PR a `develop` o `main` | gitflow, gitleaks + trivy, HTML (W3C Nu), `stylelint`, CSP en `_headers`, preview en `<rama>.fixcomap-landing.pages.dev` |
| Merge a `develop` | lo anterior + preview en `develop.fixcomap-landing.pages.dev` |
| Merge a `main` | producción tras aprobar el environment `production`, con smoke test de https://fixcomap.com |

El único check obligatorio es `cicd / ok`. Si un deploy falla, Pages sigue sirviendo la versión
anterior.

Flujo de ramas (GitFlow):

```sh
git switch develop && git pull && git switch -c feature/<cambio>
# ... cambios, commits "<tipo>(<ámbito>): <descripción>" ...
gh pr create --base develop --fill          # checks en verde, sin aprobación; mirar la preview
# para publicar:
git switch develop && git pull && git switch -c release/AAAA-MM-DD
gh pr create --base main --fill             # 1 aprobación (o bypass de admin)
# tras el merge: PR de main a develop (back-merge)
```

## Desarrollo local

Sin build: `public/` se publica tal cual.

```sh
npm ci
python3 -m http.server -d public 8000      # http://localhost:8000
npm run lint:css                           # stylelint, igual que en CI
```

| Fichero | Qué es |
|---|---|
| `public/index.html`, `public/style.css` | la página |
| `public/_headers` | cabeceras de Pages (CSP estricta: sin JS) |
| `public/robots.txt` | indexable |
| `public/assets/` | fotos y CVs; nombres exactos en `public/assets/README.md` |
| `package.json` | `stylelint` (CI) y `wrangler` (rollback en local); no se publica |

Contacto: `contacto@fixcomap.com` (Cloudflare Email Routing).
