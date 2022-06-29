

## Install Minikube
```terminal
minikube config set memory 4000
minikube config set cpus 4
minikube config set driver docker
minikube start
minikube addons enable ingress
```

## Install Postgres
```terminal
kubectl apply -f devops/postgres.yaml
```

## Install skaffold

## Run api
```terminal
skaffold dev
```

### Check with browser
```terminal
minikube ip
```
http://192.168.49.2:30764/.redwood/functions/graphql




## Run web
```terminal
skaffold dev -p web
```

### Check with browser
```terminal
minikube ip
```

## Certificate - optional

```terminal
kubectl create namespace cert-manager
```

```terminal
kubectl apply --validate=false -f https://github.com/cert-manager/cert-manager/releases/download/v1.8.0/cert-manager.yaml
```

```terminal
kubectl get pods -n cert-manager
```

### Change *spec.acme.email* in *devops/stage-certificate.yaml*
```terminal
kubectl apply -f devops/stage-certificate.yaml
```

```terminal
kubectl get clusterissuer
```

### https://pagekite.net/

```terminal
curl -s https://pagekite.net/pk/ |sudo bash
```

```terminal
python3 /usr/local/bin/pagekite.py --logfile=/home/arturas/Downloads/log.txt http://192.168.49.2 4arturas.pagekite.me
```

https://4arturas.pagekite.me/

```terminal
watch kubectl get certificate
```

### If state is False check what is wrong
```terminal
kubectl describe certificate echo-tls
```

## Argo CD MINIKUBE
```terminal
kubectl create namespace argocd
```
```terminal
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```
```terminal
wget https://github.com/argoproj/argo-cd/releases/download/v2.4.2/argocd-linux-amd64
```
```terminal
mv argocd-linux-amd64 argocd
```
```terminal
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
```
```terminal
kubectl port-forward svc/argocd-server -n argocd 8080:443
```
```terminal
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
```
```terminal
sudo bash -c 'echo -n “192.168.49.2    argocd.sys” > /etc/hosts'
```
```terminal
kubectl apply -f devops/ingress-argocd.yaml -n argocd
```

### Login with username *admin*
https://localhost:8080


## Docker
### Generate token https://github.com/settings/tokens
```terminal
docker build -f DockerfileAPI -t api .
docker tag api ghcr.io/4arturas/api
export CR_PAT=token...WqZpbOVMFWQXaJxUQ...token
echo $CR_PAT | docker login ghcr.io -u 4arturas --password-stdin
docker push ghcr.io/4arturas/api
```

# k3s
```terminal
curl -sfL https://get.k3s.io | sh -
```
```terminal
sudo chmod 644 /etc/rancher/k3s/k3s.yaml
```
```terminal
export KUBECONFIG=/etc/rancher/k3s/k3s.yaml
```
## k3s traefik dashboard
```terminal
kubectl apply -f devops/k3s-traefik-dashboard.yaml
```
http://traefik.localhost/

### Login with username *admin*
https://localhost:8080

## ArgoCD k3s
```terminal
kubectl create namespace argocd
```
```terminal
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```
```terminal
kubectl get all -n argocd
```
```terminal
kubectl apply -f devops/argocd-nodeport.yaml
```
### Login with username *admin*
http://localhost:30007
```terminal
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
```




```terminal
alias kubectl="sudo k3s kubectl"
```

