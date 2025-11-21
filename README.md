# manifests-cloud-ops

Repository chứa các Helm manifests/Charts để triển khai các ứng dụng (API, mlflow, ...).

## Mục đích

Kho lưu các chart/values để quản lý manifest (git as single source of truth). Workflow GitHub Actions trong `.github/workflows/update-helm.yml` dùng để cập nhật image tag trong `values/api/values.yaml` và push thay đổi trở lại `main`.

## Cấu trúc chính

- `apps/` — chart tổng (umbrella) và templates của root app.
- `values/` — các chart con và `values.yaml` của từng service:
  - `values/api/values.yaml` — file được workflow cập nhật image `tag`.
  - `values/mlflow/*` — chart cho mlflow

## GitHub Actions: `update-helm.yml`

Tóm tắt hành vi:
- Trigger:
  - `workflow_dispatch` (manual) với input `image_tag`.
  - `repository_dispatch` event với `client_payload.image_tag`.
- Workflow sẽ:
  1. Checkout repo (dùng `secrets.PAT_TOKEN`).
  2. Lấy tag từ input hoặc payload.
  3. Thay thế `tag:` trong `values/api/values.yaml` bằng tag mới.
  4. Commit & push thay đổi về `main` bằng cách dùng PAT.
