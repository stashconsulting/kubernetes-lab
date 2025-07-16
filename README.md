# Local Enterprise cluster

## Setup

1. Install the metric server
```
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
kubectl patch deployment metrics-server -n kube-system --patch "$(cat metric-server-patch.yaml)"
kubectl patch deployment metrics-server -n kube-system --type="json" --patch-file metric-server-patch.json
```

2. Setup cilium
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
# References
- https://github.com/kubernetes-sigs/cloud-provider-kind?tab=readme-ov-file#allowing-load-balancers-access-to-control-plane-nodes
- https://medium.com/devops-techable/ditch-docker-desktop-set-up-kubernetes-on-macos-using-podman-kind-89eb39c3bd5b
- https://docs.cilium.io/en/stable/installation/kind/
- https://github.com/kubernetes-sigs/metrics-server
- https://gist.github.com/sanketsudake/a089e691286bf2189bfedf295222bd43