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

To enable this for Applications in Argo, patch the argocd-cm with 
```shell
kubectl --kubeconfig=config -n argocd patch cm argocd-cm --type merge -p '{"data":{"kustomize.buildOptions":"--enable-helm"}}'
```
to enable helm charts inside of kustomize

the corresponding Application.yaml looks like so:

apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: kustomize-x-helm
  namespace: argocd
spec:
  destination:
    namespace: wordpress
    server: https://kubernetes.default.svc
  project: default
  source:
    kustomize:
      namespace: wordpress
    path: kustomize-x-helm/dev/
    repoURL: https://github.com/DrNaughtyDog/wordpress-helm-kustomize.git
    targetRevision: HEAD
  syncPolicy:
    retry:
      backoff:
        duration: 5s
        factor: 2
        maxDuration: 3m0s
      limit: 2
    syncOptions:
    - CreateNamespace=true
    - PruneLast=true

