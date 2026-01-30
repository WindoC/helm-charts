# Community Helm Charts for Docker-First Projects on Kubernetes

This repository provides Helm charts for popular tools and services that officially document only Docker or Docker Compose deployments. It translates those Docker-centric setups into practical, reusable Kubernetes Helm charts, based on real-world usage and production experience.

## Repo URL

`https://windoc.github.io/helm-charts`

## Charts

| Chart | Description | Version | App Version |
| --- | --- | --- | --- |
| [`guacamole`](charts/guacamole/README.md) | Guacamole + Postgres (from repo manifests) | `0.1.2` | `1.6.0` |
| [`n8n`](charts/n8n/README.md) | n8n + postgres (from repo manifests) | `0.1.1` | `2.4.7` |

## Install

```bash
helm repo add windoc https://windoc.github.io/helm-charts
helm repo update windoc
helm search repo windoc

# example to install one of the application with this repo
helm install guacamole windoc/guacamole -n guacamole
```
