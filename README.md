# Helm Chart Repository (GitHub Pages)

This repo is a Helm **Chart Repository** served via GitHub Pages.

## Repo URL

`https://windoc.github.io/helm-charts`

## Add/Update chart (Option A)

From this repo root (`helm/`):

```bash
helm package ./charts/guacamole -d repo
helm repo index repo --url https://windoc.github.io/helm-charts
```

Commit and push the contents of `repo/` to the `gh-pages` branch (or enable Pages for that branch).

## Install

```bash
helm repo add windoc https://windoc.github.io/helm-charts
helm repo update
helm install guacamole windoc/guacamole -n guacamole
```
