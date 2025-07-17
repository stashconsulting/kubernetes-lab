# Local Enterprise cluster

## Setup


### Tools

- helm
- argocd
- metric server

### Instalation

> if you want to skip the step by step setup just run: `kubectl apply -k .`

*Setup ArgoCD*

- Install argo cli

`sudo port install argocd`

- Install argo
```
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
```

*Setup cilium*
```
kubectl cluster-info --context kind-kind
helm repo add cilium https://helm.cilium.io/
podman pull quay.io/cilium/cilium:v1.17.6
kind load docker-image quay.io/cilium/cilium:v1.17.6
helm install cilium cilium/cilium --version 1.17.6 \
   --namespace kube-system \
   --set image.pullPolicy=IfNotPresent \
   --set ipam.mode=kubernetes

```

## Troubleshooting

### `error: Metrics API not available`

```
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
kubectl patch deployment metrics-server -n kube-system --type="json" --patch-file patches/metric-server-patch.json
```

or use the kustomize:
https://github.com/kubernetes-sigs/kustomize/issues/5049#issuecomment-1433601950

`kubectl apply -k .`

> https://medium.com/@cloudspinx/fix-error-metrics-api-not-available-in-kubernetes-aa10766e1c2f

### Argo not registering clusters:
https://argo-cd.readthedocs.io/en/stable/developer-guide/running-locally/

# References
- https://github.com/kubernetes-sigs/cloud-provider-kind?tab=readme-ov-file#allowing-load-balancers-access-to-control-plane-nodes
- https://medium.com/devops-techable/ditch-docker-desktop-set-up-kubernetes-on-macos-using-podman-kind-89eb39c3bd5b
- https://docs.cilium.io/en/stable/installation/kind/
- https://github.com/kubernetes-sigs/metrics-server
- https://gist.github.com/sanketsudake/a089e691286bf2189bfedf295222bd43