# Kubernetes Multi-Namespace Deployment Guide

## Overview
This guide demonstrates how to deploy two applications (Nginx and MySQL) across separate namespaces in Kubernetes.
It includes namespace creation, deployment, service exposure, scaling, and resource quota management.

---

## 1. Create Namespace for Web Application

```bash
kubectl create namespace web-app
kubectl get namespaces
```

## 2. Deploy Nginx

Create a deployment file `nginx-deployment.yaml`:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  namespace: web-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
        resources:
          requests:
            cpu: "100m"
            memory: "128Mi"
          limits:
            cpu: "250m"
            memory: "256Mi"
```

Apply and verify:
```bash
kubectl apply -f nginx-deployment.yaml
kubectl get pods -n web-app
```

## 3. Expose Nginx with NodePort Service

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
  namespace: web-app
spec:
  type: NodePort
  selector:
    app: nginx
  ports:
  - port: 80
    targetPort: 80
    nodePort: 30080
```

Apply and test:
```bash
kubectl apply -f nginx-service.yaml
kubectl get svc -n web-app
curl http://<Node-IP>:30080
```

## 4. Scale Nginx Deployment

```bash
kubectl scale deployment nginx-deployment --replicas=4 -n web-app
kubectl get pods -n web-app
```

## 5. Apply Resource Quota

```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: webapp-quota
  namespace: web-app
spec:
  hard:
    pods: "5"
    requests.cpu: "500m"
    requests.memory: "512Mi"
    limits.cpu: "1"
    limits.memory: "1Gi"
```

Apply and describe:
```bash
kubectl apply -f webapp-quota.yaml
kubectl describe resourcequota webapp-quota -n web-app
```

## 6. Create Database Namespace

```bash
kubectl create namespace db-app
kubectl get namespaces
```

## 7. Create MySQL Secret from .env

Prepare a file named `mysql.env`:
```
MYSQL_ROOT_PASSWORD=ahmed1
MYSQL_DATABASE=mydb
```

Create a Secret:
```bash
kubectl create secret generic mysql-secret --from-env-file=mysql.env -n db-app
```

## 8. Deploy MySQL Using the Secret

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mysql-deployment
  namespace: db-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
      - name: mysql
        image: mysql:latest
        ports:
        - containerPort: 3306
        envFrom:
        - secretRef:
            name: mysql-secret
```

Apply:
```bash
kubectl apply -f mysql-deployment.yaml
kubectl get pods -n db-app
```

## 9. Verify All Resources

```bash
kubectl get all -n web-app
kubectl get all -n db-app
kubectl get quota -n web-app
```

---
**End of Guide**
