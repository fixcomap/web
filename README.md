# fixcomap/web

Landing de [fixcomap.com](https://fixcomap.com). HTML/CSS estático, sin framework ni build, con CSP estricta (sin JS).

## Desarrollo local

```sh
npm ci
python3 -m http.server -d public 8000      # http://localhost:8000
npm run lint:css                           # stylelint, igual que en CI
```

| Fichero | Qué es |
|---|---|
| `public/index.html`, `public/style.css` | la página |
| `public/_headers` | cabeceras de seguridad |
| `public/assets/` | fotos y CVs (nombres en `public/assets/README.md`) |

## Despliegue

Pipeline estándar de [fixcomap/platform](https://github.com/fixcomap/platform): PR → checks;
`develop` → preview privada; `main` → producción con aprobación.
