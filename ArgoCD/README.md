# ArgoCD NGINX Deployment Lab

## Objective
Install ArgoCD on a Kubernetes cluster and deploy an NGINX application using YAML manifests from the GitHub repository

## Lab Requirements
- A running Kubernetes cluster (Minikube or any other environment)
- kubectl installed and configured
- A web browser to access the ArgoCD GUI
- Active internet connection

## Lab Tasks

### Task 1 – Install ArgoCD
1. Create a namespace for ArgoCD:
```bash
kubectl create namespace argocd
```
2. Deploy the official ArgoCD manifests:
```bash
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```
3. Verify the pods:
```bash
kubectl get pods -n argocd
```

### Task 2 – Access ArgoCD Web UI
1. Forward the argocd-server port:
```bash
kubectl port-forward svc/argocd-server -n argocd 8080:443
```
2. Access the web interface at `https://localhost:8080`
3. Retrieve the admin password:
```bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```
4. Log in using:
   - Username: `admin`
   - Password: retrieved password

### Task 3 – Create an Application (GUI Only)
1. Open ArgoCD dashboard.
2. Click "NEW APP".
3. Configure the application:
   - Application Name: `nginx-app`
   - Project: `default`
   - Repository URL: `https://github.com/abdelrahmanonline4/lab-senior`
   - Revision: `HEAD` or `main`
   - Path: `.`
   - Cluster: `https://kubernetes.default.svc`
   - Namespace: `default`
4. Click Create.

### Task 4 – Sync the Application
1. Select the application from the dashboard.
2. Click Sync to deploy the manifests.

### Task 5 – Verify the Deployment
1. Check the application state in the GUI; it should be Synced and Healthy.
2. Verify resources using kubectl (optional):
```bash
kubectl get all -n default
```
3. Access the NGINX service through the exposed port.

## Expected Deliverables
- A working ArgoCD dashboard showing the deployed NGINX application.
- Application state marked as Synced and Healthy.
![](img/result.png)
