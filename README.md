# black_box

```
kind get clusters
kind delete cluster -n argocd-cluster
kind create cluster --config kind-cluster-config.yaml --name argocd-cluster
kubectl cluster-info --context kind-argocd-cluster
```

```
kubectl create namespace argocd
helm repo add argo https://argoproj.github.io/argo-helm
helm repo update
helm install argocd argo/argo-cd --namespace argocd --set server.service.type=LoadBalancer
```

```
kubectl port-forward svc/argocd-server -n argocd 8080:80 --address 0.0.0.0
```

```
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
argocd login localhost:8080 --username admin --password rely37hMDw0mGK8T --insecure
argocd repo add git@github.com:ippanpeople/black_box.git --ssh-private-key-path /root/.ssh/id_ed25519
kubectl apply -f argocd.yaml
kubectl apply -f argocd/dev-apps/app-of-apps.yaml
argocd app sync app-of-apps --force
```

```
kubectl port-forward svc/prometheus-blackbox-exporter -n monitoring 9115:9115 --address 0.0.0.0
kubectl port-forward svc/kube-prometheus-stack-alertmanager -n monitoring 8888:9093 --address 0.0.0.0
```
