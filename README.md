# Deployment with Helm

This guide shows how to install [Data Mesh Manager](https://datamesh-manager.com/) to Kubernetes using a Helm chart.

## Prerequisites

- A managed postgres database (running Postgres in Kubernetes is not recommended) with pg_vector extension available
- An SMTP server for transactional emails

## Installation

Clone this repository
```
git clone git@github.com:datamesh-manager/datamesh-manager-helm.git
cd datamesh-manager-helm
```

Create required secrets via bash or any secrets-management mechanism you prefer.

```bash
kubectl create namespace datamesh-manager
```

```bash
kubectl create secret -n datamesh-manager docker-registry datamesh-manager-registry \
  --docker-server=ghcr.io \
  --docker-username=<provided-username> \
  --docker-password=<provided-password> 

kubectl create secret -n datamesh-manager generic datamesh-manager-database \
  --from-literal=username=<database-username> \
  --from-literal=password=<database-password>

kubectl create secret -n datamesh-manager generic datamesh-manager-smtp \
  --from-literal=username=<smtp-username> \
  --from-literal=password=<smtp-password>
```

Adjest the `values.yaml` to your needs.

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


## Access

The application is now accessible at the service endpoint under port 8080.
Example: http://datamesh-manager.<your-dns-zone>:8080 or http://<service-ip-address>:8080

## Ingress

It is highly recommended to add an ingress with TLS protection that routes to the service.
This Helm chart does not include an ingress resource, as the ingress and TLS configuration depends on your hoster and cluster configuration.

## Support

https://support.datamesh-manager.com



