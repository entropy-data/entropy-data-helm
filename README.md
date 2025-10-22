# Deployment with Helm

This guide shows how to install [Entropy Data](https://entropy-data.com/) to Kubernetes using a Helm chart.

## Prerequisites

- A managed postgres database (running Postgres in Kubernetes is not recommended) with pg_vector extension available
- An SMTP server for transactional emails

## Installation

Clone this repository
```
git clone git@github.com:entropy-data/entropy-data-helm.git
cd entropy-data-helm
```

Create a namespace for entropy-data in Kubernetes:

```bash
kubectl create namespace entropy-data
```

Create a secret with the provided container registry credentials:

```bash
kubectl create secret -n entropy-data docker-registry entropy-data-registry \
  --docker-server=ghcr.io \
  --docker-username=<provided-username> \
  --docker-password=<provided-password> 
```

Create a secret with the Postgres credentials:

```bash
kubectl create secret -n entropy-data generic entropy-data-database \
  --from-literal=username=<database-username> \
  --from-literal=password=<database-password>
```

Create a secret with the SMTP credentials:

```bash
kubectl create secret -n entropy-data generic entropy-data-smtp \
  --from-literal=username=<smtp-username> \
  --from-literal=password=<smtp-password>
```

If you want to enable Azure SSO authentication, create a secret with your application client secret:

```bash
kubectl create secret -n entropy-data generic entropy-data-azure-sso \
  --from-literal=client-id=<azure-client-id> \
  --from-literal=client-secret=<azure-client-secret>
```


Adjust the `values.yaml` to your needs.

Use Helm to install Entropy Data to Kubernetes:

```bash
helm install -n entropy-data --create-namespace entropy-data .
```

## Upgrade

```bash
helm upgrade -n entropy-data entropy-data .
```

## Uninstallation

```bash
helm uninstall -n entropy-data entropy-data
```


## Access

The application is now accessible at the service endpoint under port 8080.
Example: http://<service-ip-address>:8080

## Ingress

It is highly recommended to add an ingress with TLS protection that routes to the service.
This Helm chart does not include an ingress resource, as the ingress and TLS configuration depends on your hoster and cluster configuration.

## Support

https://support.entropy-data.com



