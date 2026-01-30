# AGENTS.md

## Repository overview
This repo hosts community Helm charts that translate Docker-first deployments into Kubernetes Helm charts. It also publishes a Helm chart repository to GitHub Pages.

## Structure
- `charts/`: Helm charts.
- `repo/`: Packaged chart artifacts and `index.yaml` for the Helm repo (published to `gh-pages`).
- `.github/workflows/helm-chart-repo.yml`: Packages charts and updates the repo index on pushes to `master`.
- `README.md`: Top-level repo documentation and install instructions.

## Conventions and expectations
- Chart values are the source of truth for names, images, and PVC settings.
- `secret.dbPassword` in `values.yaml` is a placeholder; do not ship real credentials.
- `postgres.initdb.enabled` should be `true` only for first-time initialization.
- Ingress defaults include nginx rewrite annotations; keep them unless the chart behavior changes.
- When changing chart logic or settings, bump that chart's `Chart.yaml` version and update the Charts list in the root `README.md`.

## Common commands
Run these from the repo root:
- Lint chart: `helm lint charts/<name>`
- Render manifests: `helm template <release> charts/<name> -f path/to/values.yaml`
- Package chart locally: `helm package charts/<name> -d repo`
- Update repo index: `helm repo index repo --url https://windoc.github.io/helm-charts`

## Release workflow
- Update `charts/<name>/Chart.yaml` version.
- Commit changes to `master`; GitHub Actions will package the chart and update `repo/index.yaml` on `gh-pages`.

## When adding or modifying charts
- Add new charts under `charts/<name>/` with `Chart.yaml`, `values.yaml`, and templates.
- Update the top-level `README.md` chart list.
- Ensure templates only reference values that exist in `values.yaml`.
- Prefer small, focused changes to templates; keep compatibility with existing values.

## Chart style guide
Use this as the default framework for new or updated charts.

### Files and layout
- `Chart.yaml`: `apiVersion: v2`, `type: application`, semver in `version`, app version in `appVersion`.
- `values.yaml`: single source of truth for names, images, resources, storage, and feature flags.
- `templates/`: one manifest per file; keep filenames explicit (deployment, statefulset, service, ingress, pvc, configmap, secret, namespace).
- `_helpers.tpl`: define common labels and a namespace helper.

### Naming and labels
- Centralize all resource names in `values.yaml` under `names.*`.
- Use `include "<chart>.commonLabels"` on every resource.
- Use `io.kompose.service` labels to match selector patterns where applicable.

### Values conventions
- Provide `*.image.repository`, `*.image.tag`, `*.image.pullPolicy`.
- Use `resources: {}` defaults; allow users to set limits/requests.
- Use `namespace.create: false` and template `namespace.yaml` when needed.
- Use feature toggles like `configMap.create`, `secret.create`, `recordings.createPvc`, `ingress.enabled`.
- Avoid hardcoded secrets; use `secret.*` values and `stringData` for Secrets.

### Templates conventions
- Always reference values via `Values` (no magic constants outside ports/path defaults).
- Wrap optional resources in conditionals (`if .Values.<flag>`).
- Use consistent selector labels between Services and workloads.
- Keep ingress annotations in values; include chart defaults if required for app routing.
- Use `toYaml | nindent` for maps (resources, annotations, accessModes).
- Prefer explicit `name` ports on Services and container ports.

## Chart framework and format
Use this as the canonical layout and content order for new charts or major edits.

### Recommended chart tree
- `charts/<name>/Chart.yaml`
- `charts/<name>/values.yaml`
- `charts/<name>/README.md`
- `charts/<name>/templates/_helpers.tpl`
- `charts/<name>/templates/deployment.yaml` (or `statefulset.yaml` if storage)
- `charts/<name>/templates/service.yaml`
- `charts/<name>/templates/ingress.yaml` (if enabled)
- `charts/<name>/templates/pvc.yaml` (if enabled)
- `charts/<name>/templates/configmap.yaml` (if enabled)
- `charts/<name>/templates/secret.yaml` (if enabled)
- `charts/<name>/templates/namespace.yaml` (if enabled)

### values.yaml format (top-to-bottom order)
- `names.*` (all resource names and component names)
- `namespace.*` (create/name)
- `images.*` or `*.image.*` blocks
- `service.*` and container port maps
- `ingress.*` (with annotations and paths)
- `resources` and `securityContext` defaults
- `persistence.*` or PVC sections
- `configMap.*`, `secret.*`, and feature flags
- `nodeSelector`, `tolerations`, `affinity`

### Template structure rules
- Start each template with `apiVersion`, `kind`, `metadata`, `spec` in that order.
- Use helpers for names/labels and avoid duplicating label maps.
- Keep one workload per file and one Service per file.
- Use `include "<chart>.fullname"` or values-based names from `names.*` consistently.
- Match selector labels between workload and Service using the same helper output.
- Keep defaults in `values.yaml`; do not inline fallbacks in templates.

### README.md format for charts
- Summary block (1–2 sentences)
- Quickstart with `helm install` and minimal values
- Values table generated/updated with any new keys
