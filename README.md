

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



