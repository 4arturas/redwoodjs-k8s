

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
python3 /usr/local/bin/pagekite.py http://192.168.49.2 4arturas.pagekite.me
```

https://4arturas.pagekite.me/

```terminal
watch kubectl get certificate
```

### If state is False check what is wrong
```terminal
kubectl describe certificate echo-tls
```
