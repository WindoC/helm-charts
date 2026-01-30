# AGENTS.md

## Repository overview
This repo hosts community Helm charts that translate Docker-first deployments into Kubernetes Helm charts. It also publishes a Helm chart repository to GitHub Pages.

## Structure
- `charts/`: Helm charts.
- `repo/`: Packaged chart artifacts and `index.yaml` for the Helm repo (published to `gh-pages`).
- `.github/workflows/helm-chart-repo.yml`: Packages charts and updates the repo index on pushes to `master`.
- `README.md`: Top-level repo documentation and install instructions.

## Conventions and expectations
- Ignore `.temp/` entirely.
- Chart values are the source of truth for names, images, and PVC settings.
- `secret.dbPassword` in `values.yaml` is a placeholder; do not ship real credentials.
- `postgres.initdb.enabled` should be `true` only for first-time initialization.
- Ingress defaults include nginx rewrite annotations; keep them unless the chart behavior changes.
- When changing chart logic or settings, bump that chart’s `Chart.yaml` version and update the Charts list in the root `README.md`.

## Common commands
Run these from the repo root:
- Lint chart: `helm lint charts/xxxxxxxx`
- Render manifests: `helm template xxxxxxxx charts/xxxxxxxx -f path/to/values.yaml`
- Package chart locally: `helm package charts/xxxxxxxx -d repo`
- Update repo index: `helm repo index repo --url https://windoc.github.io/helm-charts`

## Release workflow
- Update `charts/xxxxxxxx/Chart.yaml` version (and `README.md` if needed).
- Commit changes to `master`; GitHub Actions will package the chart and update `repo/index.yaml` on `gh-pages`.

## When adding or modifying charts
- Add new charts under `charts/<name>/` with `Chart.yaml`, `values.yaml`, and templates.
- Update the top-level `README.md` chart list.
- Ensure templates only reference values that exist in `values.yaml`.
- Prefer small, focused changes to templates; keep compatibility with existing values.
