# Helm Chart Repository (GitHub Pages)

This repo is a Helm **Chart Repository** served via GitHub Pages.

## Repo URL

`https://windoc.github.io/helm-charts`

## Charts

| Chart | Description | Version | App Version |
| --- | --- | --- | --- |
| [`guacamole`](charts/guacamole/README.md) | Guacamole + Postgres (from repo manifests) | `0.1.0` | `1.6.0` |

## Install

```bash
helm repo add windoc https://windoc.github.io/helm-charts
helm repo update
helm install guacamole windoc/guacamole -n guacamole
```
