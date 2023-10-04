## Deploy traefik, SSL and sample app

- Install Traefik
```
kubectl create namespace traefik-network
helm repo add traefik https://traefik.github.io/charts
helm repo update
helm upgrade --install traefik traefik/traefik -n traefik-network -f traefik/values.yaml
```

- Install cert-manager
```
kubectl create namespace cert-manager
kubectl apply --validate=false -f https://github.com/cert-manager/cert-manager/releases/download/v1.13.0/cert-manager.crds.yaml
helm template cert-manager jetstack/cert-manager --namespace cert-manager --version v1.13.0 | kubectl apply -f -
```

- Configure SSL
```
kubectl apply -k ssl
```