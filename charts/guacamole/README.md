# Guacamole Helm Chart

This chart installs Guacamole (with `guacd`) and PostgreSQL.

## Important settings

Update these in `values.yaml` before installing:

- **Database password**: `secret.dbPassword` (default is a placeholder)
- **Guacamole configuration**: `configMap.data` (you can add any Guacamole env vars here)
- **Ingress**: `ingress.*` (host, TLS secret, ingress class)
- **Storage**:
  - Postgres PVC: `postgres.persistence.*`
  - Recordings PVC: `recordings.*`

## Install

```bash
helm repo add windoc https://windoc.github.io/helm-charts
helm repo update
helm install guacamole windoc/guacamole -n guacamole
```
