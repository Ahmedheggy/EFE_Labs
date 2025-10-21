README - MySQL Deployment Lab

Objective:
Deploy a MySQL database on a dedicated node using Kubernetes best practices.

Step 1 – Node Preparation
Apply a taint to isolate the database node:
kubectl taint nodes minikube db=only:NoSchedule ## to untaint kubectl taint nodes minikube db=only:NoSchedule-
Step 2 – Configuration Files
Create two files:
- config.txt → MySQL configuration (e.g., port, bind-address)
- secret.txt → MySQL credentials (e.g., root password, user, database)

Step 3 – Kubernetes Resources
1. Create ConfigMap and Secret:
kubectl create configmap mysql-config --from-env-file=config.txt
kubectl create secret generic mysql-secret --from-env-file=secret.txt

2. Create Persistent Volume (PV) and Persistent Volume Claim (PVC).

3. Create Deployment named mysql-deployment using image mysql:8.0.
   - Load environment variables from ConfigMap and Secret.
   - Mount the PVC to /var/lib/mysql.
   - Add a Toleration for key=db, value=only, effect=NoSchedule.## in deployment yaml file

4. Create a Service named mysql-service of type ClusterIP (port 3306).

– Verification
kubectl get all,pv,pvc -o wide

Check:
- Pod runs on node minikube.
- PVC is bound to PV.
- ConfigMap and Secret are loaded.
- Service is created successfully.
