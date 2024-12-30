# 📘 Kargo Deployment Example

## **Important Things**

1. Ensure you have the following tools deployed in your cluster:
   - **ArgoCD**
   - **Argo Rollouts**
   - **Cert-Manager**

2. Install Kargo using Helm by following the official guide:
   [Installing Kargo](https://docs.kargo.io/how-to-guides/installing-kargo/)

---

## 🔖 **Scenario Overview**

This example demonstrates a **"slightly realistic"** scenario where there are **two clusters**:

- **Cluster 1**: Used for testing and UAT (User Acceptance Testing).
- **Cluster 2**: Dedicated to production.

> **Note for the OG'S**: Ideally, there should be one cluster per environment, but to simplify, we are combining testing and UAT into one.

### 📦 **Images Used**

The deployment utilizes two simple **Nginx images** stored in my **GitHub Packages**. You can find the corresponding `Dockerfile` in the `./dockerfiles` directory.

### 🔄 **Promotion Strategy**

- The promotion strategy ensures that updates always target the **latest version** following **SemVer** (Semantic Versioning).
- The process covers updates both for:
  - **New image versions** in the repository.
  - **New Git manifests** added to the source control.

---

## 🗂 **Project Structure**

```
.
├── README.md              
├── argocd-apps-of-apps     # ArgoCD App of Apps pattern for managing apps with different clusters
│   ├── application.yaml    # Main ArgoCD Application to deploy all apps
│   ├── apps-one            # Subdirectory for environment-specific applications
│   │   ├── prod-us-east-1  # ArgoCD Application for production in US East 1
│   │   │   └── application.yaml
│   │   ├── prod-us-west-1  # ArgoCD Application for production in US West 1
│   │   │   └── application.yaml
│   │   ├── test            # ArgoCD Application for testing
│   │   │   └── application.yaml
│   │   └── uat             # ArgoCD Application for UAT
│   │       └── application.yaml
│   └── kustomization.yaml  # Kustomize configuration for the Appof Apps
├── charts                  # Custom easy Helm charts for the deployment
│   └── helm-chart-own      # Custom Helm chart for this project (ignore this its not need to check or review)
│       ├── Chart.yaml       
│       ├── templates       
│       │   ├── _helpers.tpl
│       │   ├── deployment1.yaml
│       │   └── deployment2.yaml
│       └── values.yaml     # Default values for the Helm chart
├── dockerfiles             # Folder where dockerfile sare stored
│   ├── Dockerfile1         
│   └── Dockerfile2        
├── kargo.yaml              # Configuration file for Kargo 
└── stages                  # Environment-specific configurations
    ├── prod-us-east-1      # Configuration for production in US East 1
    │   ├── kustomization.yaml
    │   ├── resources       # Additional resources for the environment
    │   │   └── resources.yaml
    │   └── values.yaml     # Values specific to this environment
    ├── prod-us-west-1      # Configuration for production in US West 1
    │   ├── kustomization.yaml
    │   ├── resources
    │   │   └── resources.yaml
    │   └── values.yaml
    ├── test                # Configuration for testing
    │   ├── kustomization.yaml
    │   ├── resources
    │   │   └── resources.yaml
    │   └── values.yaml
    └── uat                 # Configuration for UAT
        ├── kustomization.yaml
        ├── resources
        │   └── resources.yaml
        └── values.yaml
```

---

## 💠 **How to Use This Example**

1. Deploy **ArgoCD**, **Argo Rollouts**, and **Cert-Manager** in your cluster.
2. Follow the [Kargo Installation Guide](https://docs.kargo.io/how-to-guides/installing-kargo/).
3. Configure your clusters as described in the scenario above.
5. Deploy Kargo configuration:
   ```bash
   kubectl apply -f kargo.yaml
   ```
6. Deploy the ArgoCD "App of Apps":
   ```bash
   kubectl apply -f argocd-apps-of-apps/application.yaml
   ```
---

