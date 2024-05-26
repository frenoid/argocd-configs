# ArgoCD
ArgoCD is a GitOps Continous Deployment tool

## Repo secrets
You can refer to [Sample Repo Secret](./repo-secret.yaml) for how to connect ArgoCD to an private SSH repository via version control

## Sealed Secrets
These sealed secrets must be applied into the cluster in order to install all these [applications](../applications/)
1. [hobby-cluster](./sealed-secret-repo-secret-hobby-cluster.yaml)
2. [normans-build-pipelines](./sealed-secret-repo-secret-normans-build-pipeline.yaml)

You can install them using these commands
```bash
kubectl apply -f "sealed-secret-repo-secret-*"
```