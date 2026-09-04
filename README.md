# k8s-gitops-app-demo

Deploys [Gitea](https://gitea.com) into a local Kubernetes cluster via
Helm, managed end-to-end by ArgoCD — push a change to this repo, ArgoCD
reconciles the cluster to match.

## Prerequisites
- A Kubernetes cluster with ArgoCD installed (see this project family's
  shared setup — install ArgoCD once per cluster).

## Deploy
```bash
kubectl apply -f argocd/gitea-app.yaml
```

## Access
```bash
kubectl -n gitops-app-demo get svc   # find the *-http service
kubectl -n gitops-app-demo port-forward svc/<that-service> 3000:3000
```
Open http://localhost:3000 — log in as `gitea_admin` / `ChangeMe123!`
(change this in `argocd/gitea-app.yaml` before any real use).

## GitOps flow
Any change pushed to `argocd/gitea-app.yaml` (e.g. bumping
`targetRevision` or editing `valuesObject`) is picked up by ArgoCD's
automated sync and applied to the cluster — no manual `kubectl` needed.
