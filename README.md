# MongoDB + Mongo Express on Kubernetes

This repository contains Kubernetes manifests to deploy a MongoDB StatefulSet with persistent storage and a Mongo Express web interface for managing your database. It includes initial data seeding and secure credentials using Kubernetes Secrets.

---

## Features

- MongoDB StatefulSet with persistent storage (1Gi)
- Headless service for MongoDB for stable pod networking
- Mongo Express Deployment with ClusterIP service
- ConfigMap for initializing `product-service-db` with sample `products`
- Kubernetes Secrets for secure credentials

---
## Deployment
After deployment, open your browser at, and you should see the Mongo Express deployment
http://localhost:8081

![mongo](mong.png)

## Prerequisites

- Kubernetes cluster (kind)
- `kubectl` installed and configured
- Optional: `helm` if you want to use charts later

---

## Deployment Instructions

### 1. Create Secrets

```bash
kubectl apply -f mongo-secret.yaml
```
This will create the MongoDB root credentials and Mongo Express login credentials.
### 2. Create ConfigMap for Initial Data

```
kubectl apply -f configmap.yaml
```
The ConfigMap seeds the MongoDB database with sample products.

#### 3. StatefulSet MongoDB

```
kubectl apply -f mongo-db.yaml
```
- The StatefulSet creates a persistent volume claim for data storage.
- MongoDB runs on port 27017 and uses the headless service mongo-headless

### 4. Mongo Express

```
kubectl apply -f mongo-express.yaml
```
- Mongo Express runs on port 8081.
- Connects to MongoDB using the headless service mongo-headless.

### Access Mongo Express
local cluster
```
kubectl port-forward svc/mongo-express-service 8081:8081
```



## Cleanup
```bash
kubectl delete -f mongo-express.yaml
kubectl delete -f mongo-db.yaml
kubectl delete -f configmap.yaml
kubectl delete -f mongo-secret.yaml
