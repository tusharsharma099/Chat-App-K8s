### Features
Real-time Messaging: Interactive UI for seamless communication.

Microservices Architecture: Separate deployments for frontend, backend, and database.

Containerized Environment: Fully Dockerized components for consistent deployment.

Kubernetes Orchestration: Managed via a dedicated chat-app namespace with persistent storage for MongoDB.

### Technology Stack
Frontend: React/Vite-based application.

Backend: Node.js/Express-based API.

Database: MongoDB with Persistent Volume (PV) and Persistent Volume Claim (PVC).


Orchestration: Kubernetes (k8s).


Infrastructure: Minikube (WSL2 environment).

### Project Structure
```text
Plaintext
/full-stack_chatApp
├── backend/            # Node.js backend source & Dockerfile
├── frontend/           # Frontend source & Dockerfile
├── k8s/                # Kubernetes Manifests
│   ├── namespace.yml
│   ├── mongodb-deployment.yml
│   ├── mongodb-service.yml
│   ├── mongodb-pv.yml
│   ├── mongodb-pvc.yml
│   ├── backend-deployment.yml
│   ├── backend-service.yml
│   ├── frontend-deployment.yml
│   └── frontend-service.yml
└── docker-compose.yml  # Local development configuration
```
### Deployment Instructions
1. Build and Push Images
From the project root, build the Docker images and push them to Docker Hub:
```

# Frontend

docker build -t tusharsharma9268/chatapp-frontend:latest ./frontend
docker push tusharsharma9268/chatapp-frontend:latest

# Backend

docker build -t tusharsharma9268/chatapp-backend:latest ./backend
docker push tusharsharma9268/chatapp-backend:latest
```
2. Apply Kubernetes Manifests
Navigate to the k8s/ directory and apply the configurations in order:
```

# Create Namespace
kubectl apply -f namespace.yml

# Setup Storage
kubectl apply -f mongodb-pv.yml
kubectl apply -f mongodb-pvc.yml

# Deploy Database
kubectl apply -f mongodb-deployment.yml
kubectl apply -f mongodb-service.yml

# Deploy Backend & Frontend
kubectl apply -f backend-deployment.yml
kubectl apply -f backend-service.yml
kubectl apply -f frontend-deployment.yml
kubectl apply -f frontend-service.yml
```
3. Accessing the Application
To access the services locally while using Minikube, use port-forwarding:
```

# Access Frontend at http://localhost:3000
kubectl port-forward service/frontend -n chat-app 3000:80

# Access Backend at http://localhost:5001
kubectl port-forward service/backend -n chat-app 5001:5001
```

### Troubleshooting
Permissions: If port-forwarding to port 80 fails due to permission denied, use a higher port (e.g., 3000).

Pod Status: Use kubectl get pods -n chat-app to monitor deployments. If a pod enters CrashLoopBackOff, use kubectl describe pod <pod-name> -n chat-app for detailed logs.
