# RaviShop AI K8s

Kubernetes, GitOps and monitoring configuration for the RaviShop AI 2026 platform. This repo holds the manifests that Argo CD would sync to a cluster, plus monitoring and security configs.

Part of the RaviShop AI 2026 project:

| Repo | Purpose |
|------|---------|
| [ravishop-ai-app](https://github.com/Ravisahu7079/ravishop-ai-app) | Flask API, AI agent, Dockerfile, CI/CD pipeline |
| [ravishop-ai-infra](https://github.com/Ravisahu7079/ravishop-ai-infra) | Terraform and Ansible |
| ravishop-ai-k8s (this repo) | Kubernetes manifests, Argo CD config, monitoring config |

## Structure

```
.
├── argocd/
│   └── application.yml          # Argo CD Application (GitOps source of truth)
└── manifests/
    ├── app/
    │   ├── configmap.yml
    │   ├── deployment.yml
    │   └── service.yml
    └── monitoring/
        ├── prometheus.yml
        ├── grafana-dashboard.json
        ├── helm-values.yml
        ├── cloudwatch-alarms.yml
        └── kube-bench-job.yml
```

## What each part does

- **argocd/application.yml**: tells Argo CD to watch this repo and keep the cluster in sync with it (GitOps).
- **manifests/app/**: Deployment, Service and ConfigMap for the RaviShop API.
- **manifests/monitoring/prometheus.yml**: Prometheus scrape configuration.
- **manifests/monitoring/grafana-dashboard.json**: Grafana dashboard for the API metrics.
- **manifests/monitoring/helm-values.yml**: Helm values for the monitoring stack.
- **manifests/monitoring/cloudwatch-alarms.yml**: CloudWatch alarm definitions.
- **manifests/monitoring/kube-bench-job.yml**: CIS Kubernetes benchmark job (kube-bench) for security auditing.

## GitOps flow

1. Change a manifest and push it to this repo.
2. Argo CD detects the change.
3. Argo CD applies it to the cluster, so Git stays the single source of truth.

## Usage

Apply the app manifests manually:

```bash
kubectl apply -f manifests/app/
```

Or register the Argo CD application:

```bash
kubectl apply -f argocd/application.yml
```

## Project status

These manifests are provided as configuration only. They were written and reviewed, but not deployed to a live cluster in this project (cost and resource limits). The live deployment ran on EC2 with Docker, Prometheus and Grafana, and is documented in the app and infra repos.

## Author

Ravi Sahu, [@Ravisahu7079](https://github.com/Ravisahu7079)
