## Install Traefik
```
kubectl --kubeconfig=test.yaml create namespace traefik-network
helm repo add traefik https://traefik.github.io/charts
helm repo update
helm --kubeconfig=test.yaml upgrade --install traefik traefik/traefik -n traefik-network -f traefik/values.yaml
```

## Install cert-manager
```
kubectl --kubeconfig=test.yaml create namespace cert-manager
kubectl --kubeconfig=test.yaml apply --validate=false -f https://github.com/cert-manager/cert-manager/releases/download/v1.13.0/cert-manager.crds.yaml
helm --kubeconfig=test.yaml template cert-manager jetstack/cert-manager --namespace cert-manager --version v1.13.0 | kubectl --kubeconfig=test.yaml apply -f -
```

## Configure SSL
```
kubectl --kubeconfig=test.yaml apply -k ssl
```

## Deploy test my-app
```
kubectl --kubeconfig=test.yaml create namespace my-app
kubectl --kubeconfig=test.yaml create configmap index.html --from-file app/index.html
kubectl --kubeconfig=test.yaml apply -f app/certificate.yaml
kubectl --kubeconfig=test.yaml apply -f app/deployment.yaml
kubectl --kubeconfig=test.yaml apply -f app/ingress.yaml
```

## Useful Commands
```
kubectl --kubeconfig=test.yaml get namespaces
kubectl --kubeconfig=test.yaml get all -n {namespace_name} -o wide
kubectl --kubeconfig=test.yaml get all --all-namespaces
kubectl --kubeconfig=test.yaml delete all --all -n {namespace_name}
helm --kubeconfig=test.yaml delete -n {namespace_name} {release_name}
```