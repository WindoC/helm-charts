# Guacamole Helm Chart

This chart installs Guacamole (with `guacd`) and PostgreSQL.

## Important settings

Update these in `values.yaml` before installing:

- **Database password**: `secret.dbPassword` (default is a placeholder)
- **Postgres initdb**: keep `postgres.initdb.enabled` set to `true` on first install, set it to `false` if the DB data already exists
- **Guacamole configuration**: `configMap.data` (set any Guacamole env vars here)
- **Ingress**: set `ingress.host` to your FQDN and add any required `ingress.annotations` (for TLS, include `cert-manager.io/cluster-issuer:` if you use cert-manager)
- **Storage**:
  - Postgres PVC: `postgres.persistence.*`
  - Recordings PVC: `recordings.*`
- **Resources**: all `resources` and `guacdResources` are intentionally unset; set these yourself if you need limits/requests

## Recordings

To enable recording support:

- Set `configMap.data.RECORDING_ENABLED` to `"true"`
- Set `recordings.createPvc` to `true`

## Configuration reference

For the official environment variable reference (including container-specific settings), see the Guacamole docs (Configuring Guacamole → Container (Docker)): https://guacamole.apache.org/doc/gug/configuring-guacamole.html. Configure any needed settings under `configMap.data.*`.

## Install with your own values file

Create your own `values.yaml` and install like this:

```bash
helm install guacamole windoc/guacamole -n guacamole -f values.yaml
```

## Install

```bash
helm repo add windoc https://windoc.github.io/helm-charts
helm repo update windoc
helm search repo windoc

# example to install from one of this helo repo
helm install guacamole windoc/guacamole -n guacamole
```
