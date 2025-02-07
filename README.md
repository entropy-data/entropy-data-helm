# Deployment with Helm

## Installation

Create required secrets via bash or any secrets-management mechanism you prefer.

```bash
kubectl create namespace datamesh-manager
```

```bash
kubectl create secret -n datamesh-manager docker-registry datamesh-manager-registry \
  --docker-server=ghcr.io \
  --docker-username=<provided-username> \
  --docker-password=<provided-password> 
```

```bash
kubectl create secret -n datamesh-manager generic datamesh-manager-database \
  --from-literal=username=<database-username> \
  --from-literal=password=<database-password>

kubectl create secret -n datamesh-manager generic datamesh-manager-smtp \
  --from-literal=username=<smtp-username> \
  --from-literal=password=<smtp-password>
```

```bash
helm install -n datamesh-manager --create-namespace datamesh-manager .
```

## Upgrade

```bash
helm upgrade -n datamesh-manager datamesh-manager .
```

## Uninstallation

```bash
helm uninstall -n datamesh-manager datamesh-manager
```
