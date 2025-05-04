Helm is a package manager for Kubernetes. It's used to manage (install, restart, stop) Kubernetes packages.
## Prometheus / Grafana / AlertManager
To install a monitoring system into a Kubernetes cluster using Helm, run the following commands:

**Get Repo Info:**
```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```

**Install Chart:**
```bash
helm install [RELEASE_NAME] [RELEASE_REPO] --namespace [RELEASE_NAME or monitoring] --create-namespace --values[or -f]=[path/to/values.yaml]
```
