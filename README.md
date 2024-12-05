    # Cosmocloud-Deploy Helm Chart

This Helm chart, `cosmocloud-deploy`, is designed to deploy a complete application stack consisting of:
1. **Backend** service
2. **Frontend** service
3. **Redis** database

The chart adheres to the specifications provided in the task, ensuring correct deployments, services, and environment variable configurations.

---

## Prerequisites

Before deploying the chart, ensure the following are set up:
- A running Kubernetes cluster.
- Helm installed (version 3.0+ recommended).
- kubectl configured to communicate with your cluster.
- Minikube (optional, if running locally).

---

## Features

- Deploys:
  - **1 Backend Pod** using the image: `shreybatra/sample-backend`
  - **1 Frontend Pod** using the image: `shreybatra/sample-frontend`
  - **1 Redis Pod** using the image: `redis`
- Exposes services:
  - Backend: **ClusterIP** type on port `8000`.
  - Frontend: **NodePort** type on port `5173`, accessible via `NodePort` `31000`.
  - Redis: **ClusterIP** type on port `6379`.
- Configures environment variables:
  - Backend: `REDIS_URI=redis://redis-svc:6379`
  - Frontend: `BACKEND_URL=http://backend-svc:8000`

---

## Installation

1. Clone the repository:
   ```bash
   git clone <your-repo-url>
   cd cosmocloud-deploy
2. Install the Helm chart:
   ```bash
   helm install testapp cosmocloud-deploy --atomic --timeout 30s
3. Verify the deployments:
    ```bash
    kubectl get pods
    kubectl get svc

## Accessing the Application
- Backend: Internal access only (ClusterIP).
- Frontend:
  - Use Minikube's built-in service exposure feature:
bash
    ```bash
    minikube service frontend-svc
    ```
    This will automatically open the frontend in your default browser.

  - Alternatively, manually access via:
    - Minikube IP: ``minikube ip``  
    - NodePort: ``31000``
    - Example URL: ``http://<MINIKUBE_IP>:31000``
- Redis: Internal access only (ClusterIP).

## Helm Chart Structure

```bash
    cosmocloud-deploy/
    ├── Chart.yaml             # Helm chart metadata
    ├── values.yaml            # Default configurations for the chart
    ├── templates/
    │   ├── deployment-backend.yaml  # Deployment for Backend
    │   ├── deployment-frontend.yaml # Deployment for Frontend
    │   ├── deployment-redis.yaml    # Deployment for Redis
    │   ├── service-backend.yaml     # Service for Backend
    │   ├── service-frontend.yaml    # Service for Frontend
    │   ├── service-redis.yaml       # Service for Redis
    │   ├── _helpers.tpl             # Template helpers (e.g., labels)
    │   └── NOTES.txt                # Post-installation notes
```
## Configuration
values.yml
``` bash

backend:
  image: "shreybatra/sample-backend"
  env:
    REDIS_URI: "redis://redis-svc:6379"
  service:
    type: ClusterIP
    port: 8000

frontend:
  image: "shreybatra/sample-frontend"
  env:
    BACKEND_URL: "http://backend-svc:8000"
  service:
    type: NodePort
    port: 5173
    nodePort: 31000

redis:
  image: "redis"
  service:
    type: ClusterIP
    port: 6379
```
## Clean-Up
To uninstall the Helm chart:

```bash
helm uninstall testapp
```
To delete all related Kubernetes resources:

```bash
kubectl delete all -l app=testapp
```

<!-- ## Debugging Tips
If you encounter issues:

1. Check the status of pods:
   ```bash
   kubectl get pods
2. Check the logs for any errors:
    ```bash
    kubectl logs <pod-name>
3. Verify service configurations:
   ```bash
   kubectl describe svc <service-name>
4. Use Minikube tunneling for NodePort issues:
    -->
