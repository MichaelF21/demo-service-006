# demo-service-006

End-to-end demo (Go 1.23 + staticcheck pinned)

> Bootstrapped by the **repo-creator** platform service.

## Run locally

```bash
go run .
# In another terminal:
curl http://localhost:8080/
curl http://localhost:8080/healthz
curl http://localhost:8080/metrics
```

## Build a container

```bash
docker build -t demo-service-006:latest .
docker run -p 8080:8080 demo-service-006:latest
```

## Endpoints

| Path       | Purpose                                  |
|------------|------------------------------------------|
| `/`        | JSON greeting (the demo functional path) |
| `/healthz` | Liveness probe                           |
| `/readyz`  | Readiness probe                          |
| `/metrics` | Prometheus metrics                       |

## CI

`.github/workflows/ci.yml` runs `go vet`, `staticcheck`, and `go test ./...` on every pull request to `main`.
Branch protection on `main` requires the `lint-and-test` check to pass and at least one approving review.

## Deploy on Kubernetes

Manifests under `deploy/k8s/` are GitOps-ready (ArgoCD / Flux can sync this directory directly).

```bash
kubectl apply -f deploy/k8s/
```

## Observability

- **Logs**: structured JSON via `slog` to stdout — pipe through your log aggregator.
- **Metrics**: Prometheus exposition at `/metrics`.
- **Health**: `/healthz` (liveness) and `/readyz` (readiness) for Kubernetes probes.
