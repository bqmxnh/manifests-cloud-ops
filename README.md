This repository contains the **Helm charts and Kubernetes manifests** used to deploy application components (such as the API service, MLflow, and related services).

## Purpose

This repository stores Helm charts and configuration values to manage Kubernetes manifests, treating **Git as the single source of truth**.

The GitHub Actions workflow located in `.github/workflows/update-helm.yml` is responsible for updating the container image tag in `values/api/values.yaml` and pushing the changes back to the `main` branch.

## Main Structure

- `apps/` — umbrella chart and root application templates.
- `values/` — subcharts and `values.yaml` files for each service:
  - `values/api/values.yaml` — file where the workflow automatically updates the container image `tag`.
  - `values/mlflow/*` — Helm chart and configuration for the MLflow service.

## GitHub Actions: `update-helm.yml`

### Workflow Summary

**Triggers**

- `workflow_dispatch` — manual trigger with input `image_tag`.
- `repository_dispatch` — triggered via API with `client_payload.image_tag`.

**Workflow steps**

1. Checkout the repository using `secrets.PAT_TOKEN`.
2. Retrieve the image tag from either the workflow input or the payload.
3. Replace the `tag:` value in `values/api/values.yaml` with the new image tag.
4. Commit and push the updated configuration back to the `main` branch using the PAT token.
