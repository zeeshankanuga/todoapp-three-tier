# 📝 TODO Application — Three-Tier Deployment on Kubernetes

This repository demonstrates the end-to-end deployment of a **Three-Tier TODO Application** built using **ReactJS (Frontend)**, **NodeJS (Backend)**, and **MongoDB (Database)** on a **Kubernetes cluster**, following best practices for containerization, environment configuration, secrets management, and service exposure.


## 🧰 Stack Overview

- **Frontend**: React.js
- **Backend**: Node.js (Express)
- **Database**: MongoDB
- **Containerization**: Docker
- **Image Registry**: Amazon ECR (Elastic Container Registry)
- **Orchestration**: Kubernetes
- **Ingress Controller**: ALB (with LoadBalancer service type)


## 📂 Project Stages

### 1. 🔧 Application Development

- **Frontend**: Created using ReactJS to handle the UI and client-side logic.
- **Backend**: Built using NodeJS and Express to manage API requests and business logic.
- **Database**: MongoDB used to persist TODO items.

### 2. 🐳 Dockerization and Pushing to Amazon ECR

- Created separate `Dockerfile` for **frontend** and **backend**.
- Built Docker images locally.
- Used AWS CLI to authenticate and push images to AWS ECR for both frontend and backend.

### 3. ☸️ Kubernetes Cluster Setup
Created and connected to a Kubernetes cluster (e.g., using EKS, Minikube, GKE, etc.).
Created separate Kubernetes manifests (`.yaml` files) for each tier:
- MongoDB: Deployment, Secret, and ClusterIP Service
- Backend: Deployment and ClusterIP Service
- Frontend: Deployment and ClusterIP Service
- Ingress: Ingress resource with LoadBalancer type to expose the frontend to the internet.

### 4. 🔐 MongoDB Setup (Database Tier)
Deployed MongoDB in the cluster using:
- A Deployment for running the MongoDB pod.
- A Secret resource to securely pass MONGO_INITDB_ROOT_USERNAME and MONGO_INITDB_ROOT_PASSWORD.
- A ClusterIP Service to expose MongoDB internally to the backend.
- The backend connects to MongoDB via the internal service name (mongo) using credentials from the secret.

### 5. ⚙️ Backend Setup (Logic Tier)
Deployed the NodeJS backend application in the cluster using:
``A Deployment to run backend pods.``
- A ClusterIP Service to expose the backend internally to the frontend.
- Configured environment variables in the backend Deployment:
- MongoDB connection string using the format: `mongodb://<username>:<password>@mongo:27017`
- The backend communicates securely with MongoDB via its internal service.

### 6. 🧑‍💻 Frontend Setup (Presentation Tier)
Deployed the ReactJS frontend using:

- A Deployment for frontend pods.
- A ClusterIP Service to expose frontend internally.
- The React app is built to make API calls to the backend using its internal service name.

### 7. 🌐 Ingress Controller & Load Balancing
Installed ALB Controller using official manifests.
```
Configured an Ingress Resource:
Routes traffic from a LoadBalancer to the frontend service.
Supports path-based routing, HTTPS termination, and domain-based access (if required).
Ingress rule exposes frontend on path / to the outside world.
Ingress allows external traffic to reach the frontend and, indirectly, the backend and database.
```
