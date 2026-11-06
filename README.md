# Local Kubernetes Application Deployment and Recovery

## Project Overview
This project demonstrates how to build, containerize, deploy, and recover a Python Flask application using Docker and Kubernetes locally with Minikube.

## Technologies Used
- Python
- Flask
- Docker
- Kubernetes
- Minikube
- kubectl
- WSL Ubuntu

## Project Structure

Local-Kubernetes-Project/
├── app/
│   ├── app.py
│   └── requirements.txt
├── kubernetes/
│   ├── deployment.yaml
│   └── service.yaml
├── Dockerfile
└── README.md

## Deployment Steps

### 1. Build Docker Image
docker build -t my-kubernetes-app:v1 .

### 2. Load Image into Minikube
minikube image load my-kubernetes-app:v1

### 3. Deploy Application
kubectl apply -f kubernetes/deployment.yaml

### 4. Create Kubernetes Service
kubectl apply -f kubernetes/service.yaml

### 5. Access Application
minikube service my-kubernetes-service --url

## Self-Healing Test

1. Check running Pods:
   kubectl get pods

2. Delete one Pod:
   kubectl delete pod POD_NAME

3. Watch Kubernetes recreate the Pod:
   kubectl get pods -w

Kubernetes automatically creates a replacement Pod to maintain the desired replica count of two.

## Expected Application Response

Hello! My Kubernetes application is running successfully!

## Project Outcome

Successfully deployed a Flask application on a local Kubernetes cluster and demonstrated Kubernetes self-healing by deleting a Pod and observing its automatic replacement.

