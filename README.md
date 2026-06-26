# azure-cicd-argocd-aks

End-to-end CI/CD pipeline using Azure DevOps, self-hosted agents, ACR, AKS, and ArgoCD GitOps for a multi-service voting application.

## 🏗️ Architecture

A multi-service voting application deployed on Azure Kubernetes Service (AKS) with a fully automated GitOps pipeline.

**Services:** Vote | Worker | Result | Redis | PostgreSQL

**Flow:**

## 🛠️ Tech Stack

| Tool | Purpose |
|------|---------|
| Azure DevOps | CI/CD Pipelines |
| Self-hosted Agent | Pipeline runner on Azure VM (Standard D2s v3) |
| Azure Container Registry (ACR) | Store Docker images |
| Azure Kubernetes Service (AKS) | Container orchestration |
| ArgoCD | GitOps continuous delivery |
| Kubernetes | Workload management |

## ⚙️ Infrastructure

- **Azure VM:** Standard D2s v3 (2 vCPUs, 8 GiB RAM) — self-hosted pipeline agent
- **AKS Cluster:** K8Cluster
- **Container Registry:** DockerImageRegistry98 (dockerimageregistry98.azurecr.io)
- **Resource Group:** AzureDevop-ResourceGroup

## 🚀 Pipelines

3 independent pipelines — one per service:

- **Vote-Pipeline** — triggers on changes to `vote/*`
- **Worker-Pipeline** — triggers on changes to `worker/*`
- **Result-Pipeline** — triggers on changes to `result/*`

Each pipeline has 3 stages:
1. **Build** — builds Docker image
2. **Push** — pushes to ACR with build ID as tag
3. **Update** — runs `updateK8sManifests.sh` to update the image tag in K8s manifest and pushes back to repo

## 🔄 GitOps Flow

ArgoCD watches the repo. When `updateK8sManifests.sh` commits the new image tag to `k8s-specifications/`, ArgoCD auto-syncs and rolls out the new version to AKS — no manual `kubectl apply` needed.

## 📸 Screenshots

### ArgoCD — Application Tree (Healthy & Synced)
![ArgoCD Tree](screenshots/argocd-tree.png)

### Azure DevOps — All Pipelines Green
![Pipelines](screenshots/pipelines.png)

### ACR — Image Tags
![ACR Tags](screenshots/acr-tags.png)

### Vote App — Live
![Vote App](screenshots/vote-app.png)