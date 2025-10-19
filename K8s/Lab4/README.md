Kubernetes Lab Summary with Commands - web-app Namespace

Step 1 - Create Namespace
Command:
kubectl create namespace web-app

Step 2 - Create Deployment
Command:
kubectl create deployment nginx-deploy --image=nginx:latest -n web-app

Step 3 - Expose Deployment (ClusterIP Service)
Command:
kubectl expose deployment nginx-deploy --port=80 --target-port=80 --type=ClusterIP -n web-app

Step 4 - Manual Scaling
Command:
kubectl scale deployment nginx-deploy --replicas=4 -n web-app

Step 5 - Horizontal Pod Autoscaler (HPA)
Command:
kubectl autoscale deployment nginx-deploy --cpu-percent=70 --min=2 --max=8 -n web-app
Note: Requires Metrics Server for CPU metrics collection.

Step 6 - ResourceQuota
YAML File: webapp-quota.yml
---
Command to apply:
kubectl apply -f webapp-quota.yml
---
Step 7 - Rolling Update (Update Deployment Image)
Command:
kubectl set image deployment/nginx-deploy nginx=nginx:1.27 -n web-app
Check rollout status:
kubectl rollout status deployment/nginx-deploy -n web-app

Step 8 - Rollback Deployment
Command:
kubectl rollout undo deployment/nginx-deploy -n web-app

Step 9 - Verification
Commands:
kubectl get pods -n web-app
kubectl get svc -n web-app
kubectl get hpa -n web-app
kubectl get resourcequota -n web-app
kubectl get all -n web-app

Concept Summary - CPU Requests vs CPU Limits
CPU Requests:
- Minimum guaranteed CPU a pod gets.
- Used by scheduler to place pods.
CPU Limits:
- Maximum CPU a pod can use before throttling.
- Enforced at runtime by the container runtime.

