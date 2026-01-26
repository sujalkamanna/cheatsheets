### 🐳 Kubernetes Architecture

               ┌────────────────────────┐
               │        kubectl          │
               └───────────┬────────────┘
                           │
        ┌──────────────────▼──────────────────┐
        │            Control Plane             │
        │--------------------------------------│
        │  API Server                          │
        │  Scheduler                           │
        │  Controller Manager                  │
        │  etcd (Cluster State DB)              │
        └──────────────────┬──────────────────┘
                           │
        ┌──────────────────▼──────────────────┐
        │              Worker Node              │
        │--------------------------------------│
        │  kubelet                             │
        │  kube-proxy                          │
        │  Container Runtime (Docker/containerd)│
        │                                      │
        │  Pod → Container → Application        │
        └──────────────────────────────────────┘


# ☸️ Kubernetes

Kubernetes orchestrates containerized applications.

👉 **Detailed notes:**  
[Kubernetes & Orchestration](../Containerization/README.md)

## Topics
- Pods, Deployments, Services
- Helm & Kustomize
- OpenShift
