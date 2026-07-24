markdown
# 🚀 Real‑Time Metrics Voting App on Kind

A comprehensive guide for setting up a **Kubernetes cluster using Kind** on a local machine or AWS EC2, deploying a **containerized voting application**, and integrating **Prometheus + Grafana** for observability.

---

## 📘 Overview

This guide covers:
- Install Docker and Kind.
- Create a Kubernetes cluster using Kind.
- Install and access kubectl.
- Build and load application images.
- Deploy the voting app with Redis, Postgres, and worker services.
- Integrate Prometheus + Grafana for real‑time observability.
- Perform load testing with Apache Benchmark.

---

## ⚙️ Step‑by‑Step Setup

### 1️⃣ Install Docker
- Download Docker Desktop for Windows from [Docker official site](https://www.docker.com/products/docker-desktop).
- Install and ensure Docker is running:
  ```powershell
  docker --version
2️⃣ Install Kind
powershell
curl.exe -Lo kind.exe https://kind.sigs.k8s.io/dl/v0.23.0/kind-windows-amd64
move kind.exe C:\Windows\System32\
kind --version
3️⃣ Install kubectl
powershell
curl.exe -LO "https://dl.k8s.io/release/v1.30.0/bin/windows/amd64/kubectl.exe"
move kubectl.exe C:\Windows\System32\
kubectl version --client
4️⃣ Check Project Structure
Ensure your repo root (k8s-kind-voting-app-main) contains:

Code
vote/
result/
worker/
k8s-specifications/
5️⃣ Build Docker Images
powershell
docker build -t vote ./vote
docker build -t result ./result
docker build -t worker ./worker
docker images
6️⃣ Create Kind Cluster
powershell
kind create cluster --name demo
7️⃣ Load Images into Kind
powershell
kind load docker-image vote:latest --name demo
kind load docker-image result:latest --name demo
kind load docker-image worker:latest --name demo
8️⃣ Deploy Kubernetes Manifests
powershell
kubectl apply -f k8s-specifications/
kubectl get pods
9️⃣ Access Applications
Vote App → http://localhost:5000

powershell
kubectl port-forward service/vote 5000:5000
Result App → http://localhost:5001

powershell
kubectl port-forward service/result 5001:5001
📊 Observability Setup
Install Helm
powershell
curl.exe -LO https://get.helm.sh/helm-v3.15.4-windows-amd64.zip
Expand-Archive helm-v3.15.4-windows-amd64.zip -DestinationPath .
move .\windows-amd64\helm.exe C:\Windows\System32\
Install Prometheus & Grafana
powershell
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
helm install kube-prometheus prometheus-community/kube-prometheus-stack
Access Grafana




# Real‑Time Metrics



A comprehensive guide for setting up a Kubernetes cluster using Kind.

## Overview

This guide covers the steps to:
- Launch on Local machine / AWS EC2 instance.
- Install Docker and Kind.
- Create a Kubernetes cluster using Kind.
- Install and access kubectl.
- Set up the Kubernetes Dashboard..
- Connect and manage your Kubernetes cluster

Step-by-Step Flow (Application real-time observability stack)
Docker
Package applications into containers.
Each service (e.g., Prometheus, Grafana) runs as a container.
Kubernetes
Orchestrates containers across nodes.
Ensures scaling, self-healing, and service discovery.
Helm Charts
Simplifies deployment of complex apps (Prometheus + Grafana).
Provides versioned, reusable templates for Kubernetes manifests.
Prometheus
Collects metrics from Kubernetes pods and nodes.
Stores time-series data (CPU, memory, network, custom app metrics).
Grafana
Connects to Prometheus as a data source.
Visualizes metrics in real-time dashboards.
Alerts can be configured for anomalies.  

## Architecture

<img width="800" height="533" alt="image" src="https://github.com/user-attachments/assets/08a94108-f5da-43ac-b296-070d687ec9b9" />


## Observability

<img width="953" height="475" alt="image" src="https://github.com/user-attachments/assets/302458f6-bf39-4ce7-a031-8aeceb1218b7" />

![Prometheus diagram](prometheus.png)

* A front-end web app in [Python](/vote) which lets you vote between two options
* A [Redis](https://hub.docker.com/_/redis/) which collects new votes
* A [.NET](/worker/) worker which consumes votes and stores them in…
* A [Postgres](https://hub.docker.com/_/postgres/) database backed by a Docker volume
* A [Node.js](/result) web app which shows the results of the voting in real time



## Resume Description

### Project Title: 

Automated Deployment of Scalable Applications on Machine with Kubernetes 

### Description: 

Led the deployment of scalable applications on Machine using Kubernetes for streamlined management and continuous integration. Orchestrated deployments via Kubernetes dashboard, ensuring efficient resource utilization and seamless scaling.

### Key Technologies:

* Machine: Infrastructure hosting for Kubernetes clusters.
* Kubernetes Dashboard: User-friendly interface for managing containerized applications.

### Achievements:

Implemented Kubernetes dashboard for visual management of containerized applications on Machine instances.
Achieved seamless scaling and high availability, supporting 99.9% uptime for critical applications.



