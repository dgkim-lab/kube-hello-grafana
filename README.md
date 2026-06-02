# kube-hello-grafana

Minimal GitOps repository for deploying standalone Grafana to an existing k3s
cluster with Argo CD, Traefik, Route53, and cert-manager.

## Prerequisites

- k3s is running and reachable with `kubectl`.
- Argo CD is installed in the `argocd` namespace.
- Traefik is available as the ingress controller.
- cert-manager has a working `ClusterIssuer` named `letsencrypt-route53-prod`.
- DNS for `grafana.k3s.dgkim.net` points to the k3s Traefik entrypoint.

## Deploy

Apply the Argo CD application:

```sh
kubectl apply -f argocd/grafana.yaml
```

Argo CD deploys Grafana from the official Grafana Helm chart into the `grafana`
namespace.

## Verify

Check the Argo CD application:

```sh
kubectl get application grafana -n argocd
```

Check Grafana resources:

```sh
kubectl get pods,svc,ingress,secret -n grafana
kubectl get certificate -n grafana
```

Open Grafana:

```text
https://grafana.k3s.dgkim.net
```

Initial local/dev login:

```text
admin / admin
```

## Notes

This repository intentionally deploys Grafana only. It does not provision
dashboards, datasources, persistent storage, or Kiali integration yet.

The fixed `admin` password is for local/dev simplicity only. Replace it with an
existing Kubernetes secret before using this outside a trusted lab environment.
