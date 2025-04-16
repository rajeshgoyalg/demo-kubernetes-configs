# Kubernetes Manifests for Multi-Cloud Flask App Deployment

This repository contains Kubernetes configuration files to deploy the sample Flask application (`demo-flask-app`) across multiple cloud Kubernetes environments, including AWS EKS, Azure AKS, Google GKE, and local clusters (e.g., Docker Desktop). The manifests are designed for easy, cloud-native deployment and learning.

---

## Repository Overview

- **Purpose:** Provide reusable, cloud-agnostic Kubernetes manifests for deploying a simple Flask app with best practices for service exposure and ingress across major cloud providers.

## Folder Structure

```
demo-kubernetes-configs/
├── demo-flask-app/
│   ├── deployment.yaml         # App Deployment manifest
│   ├── service.yaml           # Service (NodePort) manifest
│   ├── ingress.yaml           # Ingress for AWS EKS (ALB)
│   ├── aks_ingress.yaml       # Ingress for Azure AKS (NGINX)
│   └── gke_ingress.yaml       # Ingress for Google GKE (GCE)
└── README.md                  # (This file)
```

---

## Prerequisites

- [kubectl](https://kubernetes.io/docs/tasks/tools/)
- Access to a Kubernetes cluster (EKS, AKS, GKE, or Docker Desktop)
- Proper `kubeconfig` for your target cluster
- (For cloud) Sufficient permissions to create deployments, services, and ingress resources

---

## Setup Instructions

1. **Clone the repo:**
   ```sh
   git clone https://github.com/your-username/demo-kubernetes-configs.git
   cd demo-kubernetes-configs/demo-flask-app
   ```
2. **Set your kubectl context:**
   ```sh
   kubectl config use-context <your-context>
   ```

---

## How to Apply Manifests

Apply the manifests in the following order:

1. **Deployment:**
   ```sh
   kubectl apply -f deployment.yaml
   ```
2. **Service:**
   ```sh
   kubectl apply -f service.yaml
   ```
3. **Ingress:**
   - For AWS EKS (ALB):
     ```sh
     kubectl apply -f ingress.yaml
     ```
   - For Azure AKS (NGINX):
     ```sh
     kubectl apply -f aks_ingress.yaml
     ```
   - For Google GKE (GCE):
     ```sh
     kubectl apply -f gke_ingress.yaml
     ```

---

## Manifest Details

- **deployment.yaml:**
  - Deploys the Flask app as a single replica pod using the container image `rajeshgoyalg/demo-flask-app:latest`.
  - Sets resource requests/limits for efficient scheduling.

- **service.yaml:**
  - Exposes the deployment using a `NodePort` service on port 80 (mapped to container port 3000).

- **ingress.yaml:**
  - AWS EKS ingress using ALB (`alb` class), with annotations for internet-facing access and group management.

- **aks_ingress.yaml:**
  - Azure AKS ingress using the `nginx` class.

- **gke_ingress.yaml:**
  - Google GKE ingress using the `gce` class for HTTP Load Balancer.

---

## Ingress Configuration by Cloud Provider

- **AWS EKS:**
  - Uses AWS ALB Ingress Controller (see `ingress.yaml`).
  - Requires ALB controller installed and IAM permissions.
  - DNS/URL assigned by AWS ALB after creation.

- **Azure AKS:**
  - Uses NGINX Ingress Controller (see `aks_ingress.yaml`).
  - Requires NGINX controller installed in the cluster.
  - External IP assigned to ingress service.

- **Google GKE:**
  - Uses GCE HTTP Load Balancer (see `gke_ingress.yaml`).
  - External IP assigned by GCP after ingress creation.

---

## Notes on Domain Mapping & LoadBalancer

- Ingress controllers provision external endpoints (DNS or IP) for public access.
- To use a custom domain, create a DNS A/CNAME record pointing to the assigned IP or DNS name.
- For local testing (e.g., Docker Desktop), you may port-forward the service:
  ```sh
  kubectl port-forward service/demo-flask-app-service 3000:80
  ```
  Then access: http://localhost:3000

---

## Tips for Local Testing

- Use [Docker Desktop](https://www.docker.com/products/docker-desktop/) or [kind](https://kind.sigs.k8s.io/) for a local Kubernetes cluster.
- You can skip ingress and use `kubectl port-forward` for quick access.
- Ensure the container image (`rajeshgoyalg/demo-flask-app:latest`) is publicly accessible, or build and push your own.

---

## Contribution & License

Contributions are welcome! Please open issues or submit PRs for improvements or new cloud provider support.

This project is licensed under the [MIT License](LICENSE).
 demo-kubernetes-configs
demo-kubernetes-configs
