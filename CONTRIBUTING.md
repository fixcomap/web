# Cómo contribuir

Mismo flujo que `fixcomap/platform`: GitFlow, `develop` por defecto. `deploy.yml` publica `develop` como
preview y `main` en producción (environment `production`).

```sh
git switch develop && git pull
git switch -c feature/nombre
# ... cambios ...
git push -u origin feature/nombre
gh pr create --base develop --fill        # 0 aprobaciones; checks en verde
# publicar:
git switch develop && git pull && git switch -c release/AAAA-MM-DD
gh pr create --base main --fill           # 1 aprobación del otro
```

Sin `VERSION` ni tags: una landing no versiona; la fecha en el nombre de la release basta.

Checks: `gitflow`, `gitleaks + trivy`, `html + css` (Nu validator, stylelint, CSP en `_headers`, assets
referenciados presentes). Commits: `<tipo>(<ámbito>): <descripción en minúsculas>`, una línea.
Actions por SHA con `# vX.Y.Z`; Renovate las actualiza los lunes.

Fotos y CVs: `public/assets/` con los nombres de `public/assets/README.md`. Fotos cuadradas < 200 KB.
