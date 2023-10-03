## How to Use kubectl with kubeconfig

- Open your file {filename}.yaml > rename contexts
- Inside VSCode > Open new terminal "Power Shell"
- Export all your yaml to kubeconfig
```
# Go to your list kubeconfig directory:
cd /path/kubeconfig

# Export all yaml to the environment KUBECONFIG:
- $env:KUBECONFIG="cluster1.yaml;cluster2.yaml"
- kubectl config view --raw > C:\Users\User\.kube\config # <- your .kube directory
```

- Use context your cluster
```
kubectl config get-contexts 
kubectl config use-context {context_name}
kubectl get node
```

<hr>

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

- Deploy test my-app
```
kubectl create namespace my-app
kubectl create configmap index.html --from-file app/index.html
kubectl apply -k apps/simple-nginx
```
<hr>

# Deploy using gitlab registry
- Make sure you installed docker and kubectl in your runner

- Create token:
https://gitlab.com/{project-path}/settings/access_tokens

- Create secret:
```
kubectl create secret docker-registry gitlab-token --docker-server=https://registry.gitlab.com --docker-username=<username> --docker-password="<personal access token>" -n <my-app>
```

- Create environment variable:
https://malgasm.com/blog/166/how-to-deploy-to-kubernetes-using-gitlab-ci

<hr>

## Useful Commands
```
# Use additonal kubeconfig yaml:
kubectl --kubeconfig=test.yaml create namespace traefik-network

# List Commands
kubectl get pod --all-namespaces
kubectl get namespaces
kubectl get all -n {namespace_name} -o wide
kubectl get all --all-namespaces
kubectl delete all --all -n {namespace_name}
helm delete -n {namespace_name} {release_name}
```