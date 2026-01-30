# n8n Helm Chart

This chart installs n8n with PostgreSQL.

## Important settings

Update these in `values.yaml` before installing:

- **Database password**: `secret.DB_POSTGRESDB_PASSWORD` (default is a placeholder)
- **n8n config**: `configMap.data.*` (set `N8N_HOST` and `WEBHOOK_URL` to your FQDN)
- **Ingress**: set `ingress.host` and optional `ingress.annotations` (for TLS, include `cert-manager.io/cluster-issuer:` if you use cert-manager)
- **Storage**:
  - n8n PVC: `persistence.*`
  - Postgres PVC: `postgres.persistence.*`
- **Resources**: `n8n.resources` and `postgres.resources` are intentionally empty by default

## Install the repo (id needed)

```bash
helm repo add windoc https://windoc.github.io/helm-charts
helm repo update windoc
helm search repo windoc
```

## Install with your own values file

Create your own `values.yaml` and install like this:

```bash
helm install n8n windoc/n8n -n n8n -f values.yaml
```

## Values

| Key | Type | Default | Description |
| --- | --- | --- | --- |
| `names.configMap` | string | `n8n-config` | ConfigMap name |
| `names.secret` | string | `n8n-secrets` | Secret name |
| `names.n8nDeployment` | string | `n8n` | n8n Deployment name |
| `names.n8nService` | string | `n8n` | n8n Service name |
| `names.n8nPvc` | string | `n8n-data` | n8n PVC name |
| `names.postgresService` | string | `postgres` | Postgres Service name |
| `names.postgresStatefulSet` | string | `postgres` | Postgres StatefulSet name |
| `namespace.create` | bool | `false` | Create namespace |
| `n8n.replicaCount` | int | `1` | n8n replica count |
| `n8n.strategy.type` | string | `Recreate` | Deployment strategy |
| `n8n.image.repository` | string | `docker.n8n.io/n8nio/n8n` | n8n image repository |
| `n8n.image.tag` | string | `2.4.7` | n8n image tag |
| `n8n.image.pullPolicy` | string | `IfNotPresent` | n8n image pull policy |
| `n8n.resources` | object | `{}` | n8n resource requests/limits |
| `postgres.replicaCount` | int | `1` | Postgres replica count |
| `postgres.image.repository` | string | `postgres` | Postgres image repository |
| `postgres.image.tag` | string | `16` | Postgres image tag |
| `postgres.image.pullPolicy` | string | `IfNotPresent` | Postgres image pull policy |
| `postgres.resources` | object | `{}` | Postgres resource requests/limits |
| `postgres.persistence.size` | string | `10Gi` | Postgres PVC size |
| `postgres.persistence.accessModes` | list | `["ReadWriteOnce"]` | Postgres PVC access modes |
| `postgres.persistence.storageClassName` | string | `""` | Postgres storage class |
| `postgres.persistence.volumeMode` | string | `Filesystem` | Postgres volume mode |
| `postgres.persistence.pvcRetentionPolicy.whenDeleted` | string | `Retain` | Retention policy on delete |
| `postgres.persistence.pvcRetentionPolicy.whenScaled` | string | `Retain` | Retention policy on scale |
| `postgres.podManagementPolicy` | string | `OrderedReady` | Pod management policy |
| `postgres.updateStrategy.type` | string | `RollingUpdate` | Update strategy |
| `postgres.updateStrategy.partition` | int | `0` | Rolling update partition |
| `service.n8n.type` | string | `ClusterIP` | n8n Service type |
| `service.n8n.port` | int | `5678` | n8n Service port |
| `service.n8n.targetPort` | int | `5678` | n8n target port |
| `service.n8n.name` | string | `http` | n8n port name |
| `service.postgres.type` | string | `ClusterIP` | Postgres Service type |
| `service.postgres.port` | int | `5432` | Postgres Service port |
| `service.postgres.targetPort` | int | `5432` | Postgres target port |
| `service.postgres.name` | string | `psql` | Postgres port name |
| `ingress.enabled` | bool | `false` | Enable ingress |
| `ingress.name` | string | `n8n-ingress` | Ingress name |
| `ingress.ingressClassName` | string | `""` | Ingress class name |
| `ingress.annotations` | object | `{}` | Extra ingress annotations |
| `ingress.host` | string | `n8n.example.com` | Ingress host |
| `ingress.path` | string | `/(.*)` | Ingress path |
| `ingress.pathType` | string | `ImplementationSpecific` | Ingress path type |
| `ingress.tls.enabled` | bool | `true` | Enable TLS |
| `ingress.tls.secretName` | string | `n8n-tls-cert` | TLS secret name |
| `persistence.createPvc` | bool | `true` | Create n8n PVC |
| `persistence.size` | string | `10Gi` | n8n PVC size |
| `persistence.accessModes` | list | `["ReadWriteOnce"]` | n8n PVC access modes |
| `persistence.storageClassName` | string | `""` | n8n storage class |
| `persistence.volumeMode` | string | `Filesystem` | n8n volume mode |
| `persistence.mountPath` | string | `/home/node/.n8n` | n8n data mount path |
| `persistence.subPath` | string | `.n8n` | n8n volume subPath |
| `configMap.create` | bool | `true` | Create ConfigMap |
| `configMap.data` | object | `{...}` | ConfigMap data |
| `secret.create` | bool | `true` | Create Secret |
| `secret.DB_POSTGRESDB_PASSWORD` | string | `db_password_change_me` | Database password |
| `nodeSelector` | object | `{}` | Node selector |
| `tolerations` | list | `[]` | Tolerations |
| `affinity` | object | `{}` | Affinity |
