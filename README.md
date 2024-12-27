# Apereo CAS - Test usecases

## Deploy Hub

### Create namespace

```bash
kubectl create ns traefik
```

### Create Hub token secret

```bash
kubectl create secret generic hub-license --from-literal=token="${HUB_TOKEN}" -n traefik
```

### deploy Traefik

```bash
helm upgrade --install traefik traefik/traefik --create-namespace --namespace traefik --values hub/hub-values.yaml
```

## Deploy Apereo CAS

```bash
helm upgrade --install cas /Users/newa/Documents/traefik/git/cas-overlay-template/helm/cas-server --create-namespace --namespace cas --values cas/cas-values.yaml
```

## Deploy app

### Create apps namespace

```bash
kubectl create ns whoami
```

### Deploy whoami, middleware and ingress

```bash
kubectl apply -f whoami/whoami.yaml
envsubst < whoami/middlewares.yaml | kubectl apply -f -
envsubst < whoami/ingress.yaml | kubectl apply -f -
```


## Deploy Consul

```bash
kubectl create consul
```

```bash
helm upgrade -i --values consul/values.yaml consul hashicorp/consul --create-namespace --namespace consul
```

```bash
envsubst < consul/ingress.yaml | kubectl apply -f -
```
