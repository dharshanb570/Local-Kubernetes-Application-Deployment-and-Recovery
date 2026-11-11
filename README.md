# Local Kubernetes Application Deployment and Recovery

## 1. Project Overview

This project demonstrates how to build, containerize, deploy, and recover a Python Flask web application using Docker and Kubernetes locally with Minikube.

The project demonstrates Kubernetes deployment, service exposure, multiple replicas, and automatic Pod recovery through Kubernetes self-healing.

## 2. Objectives

- Understand Docker containerization.
- Deploy a Flask application using Kubernetes.
- Configure Kubernetes Deployments and Services.
- Run multiple application replicas.
- Demonstrate automatic Pod recovery.
- Access the application through a Kubernetes Service.

## 3. Technologies Used

- Python
- Flask
- Docker
- Kubernetes
- Minikube
- kubectl
- WSL2 Ubuntu
- Windows 11

## 4. Project Structure

```text
Local-Kubernetes-Project/
├── app/
│   ├── app.py
│   └── requirements.txt
├── kubernetes/
│   ├── deployment.yaml
│   └── service.yaml
├── Dockerfile
└── README.md
```

## 5. Application Details

The application is developed using Python Flask.

It runs on port 5000 and returns the following message:

```text
Hello! My Kubernetes application is running successfully!
```

## 6. Deployment Procedure

### Step 1: Start Minikube

```bash
minikube start --driver=docker
```

### Step 2: Build Docker Image

```bash
docker build -t local-k8s-demo:v1 .
```

### Step 3: Load Image into Minikube

```bash
minikube image load local-k8s-demo:v1
```

### Step 4: Deploy the Application

```bash
/usr/bin/kubectl apply -f kubernetes/deployment.yaml
```

### Step 5: Create Kubernetes Service

```bash
/usr/bin/kubectl apply -f kubernetes/service.yaml
```

### Step 6: Verify Deployment

```bash
/usr/bin/kubectl get deployments
```

### Step 7: Check Running Pods

```bash
/usr/bin/kubectl get pods -l app=local-k8s-app
```

### Step 8: Check Kubernetes Service

```bash
/usr/bin/kubectl get svc local-k8s-service
```

### Step 9: Access the Application

```bash
minikube service local-k8s-service --url
```

Keep the terminal running because the Docker driver on Linux uses a temporary service tunnel.

Use the displayed URL in a second terminal:

```bash
curl http://127.0.0.1:PORT
```

Replace PORT with the port displayed by Minikube.

## 7. Kubernetes Self-Healing Demonstration

The project demonstrates Kubernetes self-healing by deleting one application Pod and observing its automatic replacement.

### Step 1: Check Running Pods

```bash
/usr/bin/kubectl get pods -l app=local-k8s-app
```

### Step 2: Select and Delete One Pod

```bash
POD=$(/usr/bin/kubectl get pods -l app=local-k8s-app -o jsonpath='{.items[0].metadata.name}')
/usr/bin/kubectl delete pod "$POD"
```

### Step 3: Watch Pod Recovery

```bash
/usr/bin/kubectl get pods -l app=local-k8s-app -w
```

Kubernetes automatically creates a replacement Pod to maintain the desired replica count of two.

This demonstrates Kubernetes self-healing through Deployment and ReplicaSet reconciliation.

## 8. Verification Results

The following results were successfully observed during project execution:

- Docker image `local-k8s-demo:v1` built successfully.
- Minikube cluster started successfully.
- Kubernetes Deployment created successfully.
- Two application Pods reached Running status.
- Deployment rollout completed successfully.
- NodePort Service created on port 30008.
- Flask application accessed successfully using curl.
- Deleted Pod automatically replaced by Kubernetes.
- Application returned to two running replicas.

## 9. Expected Application Output

```text
Hello! My Kubernetes application is running successfully!
```

## 10. Project Outcome

Successfully deployed a Python Flask application on a local Kubernetes cluster using Docker and Minikube.

The project demonstrated containerization, Kubernetes Deployment, Service configuration, multiple replicas, and automatic Pod recovery.

**Note:** This project demonstrates Pod-level recovery in a single-node local Kubernetes cluster. It does not demonstrate multi-node high availability or full disaster recovery.
