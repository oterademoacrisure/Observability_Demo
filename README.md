You can copy‑paste this directly into your GitHub repo:

markdown
# 🚀 Real‑Time Metrics — Observability Demo Voting App on Kind

A comprehensive guide for setting up a **Kubernetes cluster using Kind** on Windows (or AWS EC2), deploying a **containerized voting application**, and integrating **Prometheus + Grafana** for observability.

---

## 📘 Overview

This guide covers:
- Launch on Local machine / AWS EC2 instance.
- Install Docker Desktop.
- Install Kind, kubectl, and Helm (with proper PATH setup).
- Create and verify a Kubernetes cluster.
- Build and load application images.
- Deploy the voting app with Redis, Postgres, and worker services.
- Set up the Kubernetes Dashboard.
- Integrate Prometheus + Grafana for real‑time observability.
- Perform load testing with Apache Benchmark.
- Cleanup instructions.

---

## 🔄 Step‑by‑Step Flow (Application Real‑Time Observability Stack)

- **[Docker](ca://s?q=Explain_Docker)** → Package applications into containers. Each service (Prometheus, Grafana, etc.) runs as a container.  
- **[Kubernetes](ca://s?q=Explain_Kubernetes)** → Orchestrates containers across nodes. Ensures scaling, self‑healing, and service discovery.  
- **[Helm Charts](ca://s?q=Explain_Helm_Charts)** → Simplifies deployment of complex apps (Prometheus + Grafana). Provides reusable templates.  
- **[Prometheus](ca://s?q=Explain_Prometheus)** → Collects metrics from Kubernetes pods/nodes. Stores time‑series data (CPU, memory, network, custom app metrics).  
- **[Grafana](ca://s?q=Explain_Grafana)** → Connects to Prometheus as a data source. Visualizes metrics in real‑time dashboards. Alerts can be configured for anomalies.  

---

## 🏗️ Architecture

<img width="800" height="533" alt="image" src="https://github.com/user-attachments/assets/08a94108-f5da-43ac-b296-070d687ec9b9" />

---

## ⚙️ Step‑by‑Step Setup

### 1️⃣ Install Docker
Download Docker Desktop for Windows from [Docker official site](https://www.docker.com/products/docker-desktop).

Verify installation:
```powershell
docker --version
2️⃣ Install Kind (Kubernetes in Docker)
Download Kind binary:

powershell
curl.exe -Lo kind.exe https://kind.sigs.k8s.io/dl/v0.23.0/kind-windows-amd64
Move to PATH:

powershell
move kind.exe C:\Windows\System32\
Verify:

powershell
kind --version
Create cluster:

powershell
kind create cluster --name demo
3️⃣ Install kubectl
Download kubectl binary:

powershell
curl.exe -LO "https://dl.k8s.io/release/v1.30.0/bin/windows/amd64/kubectl.exe"
Move to PATH:

powershell
move kubectl.exe C:\Windows\System32\
Verify:

powershell
kubectl version --client
4️⃣ Verify Cluster
powershell
kubectl cluster-info
kubectl get nodes
5️⃣ Build Application Images
powershell
docker build -t vote ./vote
docker build -t result ./result
docker build -t worker ./worker
docker images
6️⃣ Load Images into Kind
powershell
kind load docker-image vote:latest --name demo
kind load docker-image result:latest --name demo
kind load docker-image worker:latest --name demo
7️⃣ Deploy Kubernetes Manifests
powershell
kubectl apply -f k8s-specifications/
kubectl get pods
kubectl get svc
8️⃣ Access Applications
Vote App

powershell
kubectl port-forward service/vote 5000:5000
→ http://localhost:5000

Result App

powershell
kubectl port-forward service/result 5001:5001
→ http://localhost:5001

📊 Observability Setup
🔹 Install Helm
Download Helm:

powershell
curl.exe -LO https://get.helm.sh/helm-v3.15.4-windows-amd64.zip
Extract:

powershell
Expand-Archive helm-v3.15.4-windows-amd64.zip -DestinationPath .
Move to PATH:

powershell
move .\windows-amd64\helm.exe C:\Windows\System32\
Verify:

powershell
helm version
🔹 Install Prometheus & Grafana
powershell
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
helm install kube-prometheus prometheus-community/kube-prometheus-stack
🔹 Access Grafana
powershell
kubectl port-forward service/kube-prometheus-grafana 3000:80
→ http://localhost:3000

Username: admin  
Password:

powershell
kubectl get secret kube-prometheus-grafana -o jsonpath="{.data.admin-password}" | ForEach-Object { [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($_)) }
🔄 Updating & Rollout
After code changes:

powershell
docker build -t vote ./vote
kind load docker-image vote:latest --name demo
kubectl rollout restart deployment vote
⚡ Load Testing
Install Apache Benchmark (ab) via Apache HTTPD distribution or package manager.

Run benchmark:

powershell
ab -n 1000 -c 50 http://localhost:5000/
📊 Observability
<img width="953" height="475" alt="image" src="https://github.com/user-attachments/assets/302458f6-bf39-4ce7-a031-8aeceb1218b7" />

[Looks like the result wasn't safe to show. Let's switch things up and try something else!]

A front‑end web app in Python ( in Bing) which lets you vote between two options

A Redis which collects new votes

A .NET ( in Bing) worker which consumes votes and stores them in…

A Postgres database backed by a Docker volume

A Node.js ( in Bing) web app which shows the results of the voting in real time
