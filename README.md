#  Managing Infrastructure for Microservices using Ansible or Terraform

This project demonstrates how to manage and deploy a microservices-based backend (built with Flask) using Minikube for Kubernetes orchestration and optionally Ansible or Terraform for infrastructure automation.

# Project Structure
├── app
│ └── backend
│ ├── app.py # Flask backend service
│ └── Dockerfile # Container specification
├── k8s
│ ├── backend-deployment.yaml # Kubernetes deployment config
│ └── backend-service.yaml # Kubernetes service config
├── ansible
│ ├── install_docker.yml # Ansible playbook to install Docker
│ ├── install_minikube.yml # Ansible playbook to install Minikube
│ └── deploy_services.yml # Playbook for deploying app (optional)
├── docs
│ └── project-overview.md # Project report and documentation
└── README.md # This file

---

# Tech Stack

- 🐳 Docker
- ☸️ Kubernetes (Minikube)
- 🐍 Python + Flask
- 📜 YAML for deployment configuration
- ⚙️ Ansible (for automation, optional)
- ☁️ Google Cloud Shell (or local VM)

---

# Setup Instructions

 Step 1: Build and Push Docker Image

```bash
cd app/backend
docker build -t flask-backend:latest .
docker tag flask-backend:latest <your-dockerhub-username>/flask-backend:latest
docker push <your-dockerhub-username>/flask-backend:latest
  --Ensure your Kubernetes deployment YAML uses the correct DockerHub image.

Step 2: Start Minikube
minikube start
Tip: If already started, skip this step.

Step 3: Apply Kubernetes Manifests
kubectl apply -f k8s/backend-deployment.yaml
kubectl apply -f k8s/backend-service.yaml

kubectl get pods
kubectl get svc
Expected output:

pgsql
Copy code
NAME                                 READY   STATUS    RESTARTS   AGE
backend-deployment-xxxxxx-xxxxx      1/1     Running   0          1m

NAME              TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
backend-service   NodePort   10.96.188.51    <none>        80:30950/TCP   2m

Step 5: Access the Application
Option 1: Minikube Service URL
bash
Copy code
minikube service backend-service --url
Open the URL (e.g., http://192.168.49.2:30950) in your browser.

Option 2: Port Forward (if Minikube URL fails)
bash
Copy code
kubectl port-forward service/backend-service 8080:80
Then visit: http://127.0.0.1:8080

Expected output:
Hello from the Flask backend!


