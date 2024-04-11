# Kustomize x Helm PoC

This folder demonstrates using Helm Charts in combination with Kustomize. The Helm Charts are installed commonly, yet the post-renderer parameter uses a simple script that utilizes Kustomize capabilities to change the namespace of all Helm Chart Resources

## Deployment

Beforehand, create the namespace referenced in the kustomization.yaml e.g. wordpress.
Then
```bash
kustomize build --enable-helm ./dev
```
or alternatively with kubectl
```bash
kubectl kustomize --enable-helm ./dev
```
