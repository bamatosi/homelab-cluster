# Homelab Kubernetes Cluster
My homelab k8s cluster started with two bare metal nodes on Lenovo ThinkPan m920q micro pc with Ubuntu Sever 24.04 installed and cluster deployed with k3s

This repo is a FluxCD gitops code to provision everything in the cluster

# Secrets
Use a [public certificate for sealed secrets](./pub-sealed-secrets.pem) to create `SealedSecrets` that can be safely stored in Git
```
# 1. Locally create Secret
kubectl -n default create secret generic basic-auth \
--from-literal=user=admin \
--from-literal=password=change-me \
--dry-run=client \
-o yaml > basic-auth.yaml

# 2. Encrypt/seal it
kubeseal --format=yaml --cert=pub-sealed-secrets.pem < basic-auth.yaml > basic-auth-sealed.yaml
```
Then commit `SealedSecret` to repo to create in the cluster and mount to the pod as any other secret

## Roadmap
[X] Storage - https://longhorn.io
[X] Sealed secrets
[] Grafana and prometheus - for monitoring
[X] Traefik - ingress controller
[] Expose externally