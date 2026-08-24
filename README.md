# 🚀 Kubernetes Monitoring Stack

<p align="center">
  <b>Containerized Application Monitoring with Kubernetes, Prometheus & Grafana</b>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Kubernetes-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white" />
  <img src="https://img.shields.io/badge/Minikube-94399E?style=for-the-badge&logo=kubernetes&logoColor=white" />
  <img src="https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white" />
  <img src="https://img.shields.io/badge/Prometheus-E6522C?style=for-the-badge&logo=prometheus&logoColor=white" />
  <img src="https://img.shields.io/badge/Grafana-F46800?style=for-the-badge&logo=grafana&logoColor=white" />
  <img src="https://img.shields.io/badge/Helm-0F1689?style=for-the-badge&logo=helm&logoColor=white" />
</p>

---

## 📌 Overview

This project demonstrates how to deploy and monitor a containerized application on a local **Kubernetes cluster using Minikube**.

The monitoring stack uses **Prometheus** for metrics collection and **Grafana** for visualization and observability.

The project covers core Kubernetes concepts including:

* Kubernetes Deployments
* Kubernetes Services
* Containerized applications
* Pod management
* Replica scaling
* Metrics collection
* Monitoring dashboards
* Kubernetes observability
* Helm-based monitoring deployment

---

## 🏗️ Architecture

```text
                    ┌──────────────────────┐
                    │   Containerized App  │
                    │       NGINX          │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │ Kubernetes Service   │
                    │      NodePort        │
                    └──────────┬───────────┘
                               │
                               ▼
              ┌────────────────────────────────┐
              │        Minikube Cluster         │
              │                                │
              │  ┌────────┐    ┌────────┐     │
              │  │ Pod 1  │    │ Pod 2  │ ... │
              │  └────────┘    └────────┘     │
              │                                │
              └────────────────┬───────────────┘
                               │
                               │ Metrics
                               ▼
                    ┌──────────────────────┐
                    │     Prometheus       │
                    │  Metrics Collection  │
                    └──────────┬───────────┘
                               │
                               │ Queries
                               ▼
                    ┌──────────────────────┐
                    │       Grafana        │
                    │ Monitoring Dashboard │
                    └──────────────────────┘
```

---

## 🛠️ Tech Stack

| Technology    | Purpose                           |
| ------------- | --------------------------------- |
| ☸️ Kubernetes | Container orchestration           |
| 🟣 Minikube   | Local Kubernetes cluster          |
| 🐳 Docker     | Container runtime                 |
| 🔥 Prometheus | Metrics collection                |
| 📊 Grafana    | Metrics visualization             |
| ⎈ Helm        | Kubernetes package management     |
| 💻 kubectl    | Kubernetes command-line interface |
| 🌐 NGINX      | Sample web application            |

---

## 📂 Project Structure

```text
kubernetes-monitoring-demo/
│
├── deployment.yaml
├── service.yaml
├── README.md
├── .gitignore
│
└── screenshots/
    ├── arch.jpeg
    ├── kube.jpeg
    ├── prometheus.jpeg
    └── scale.jpeg
```

---

# ⚙️ Prerequisites

Make sure the following tools are installed:

* Docker
* Minikube
* kubectl
* Helm
* Git

Verify the installation:

```bash
docker --version
minikube version
kubectl version --client
helm version
git --version
```

---

# 🚀 Getting Started

## 1️⃣ Start Minikube

Start a local Kubernetes cluster using Docker:

```bash
minikube start --driver=docker
```

Verify the cluster:

```bash
kubectl get nodes
```

Expected output:

```text
NAME       STATUS   ROLES           AGE
minikube   Ready    control-plane   ...
```

---

## 2️⃣ Deploy the Application

Apply the Kubernetes Deployment:

```bash
kubectl apply -f deployment.yaml
```

Check the Pods:

```bash
kubectl get pods
```

You should see two NGINX replicas running.

---

## 3️⃣ Create the Kubernetes Service

Apply the Service configuration:

```bash
kubectl apply -f service.yaml
```

Verify:

```bash
kubectl get services
```

Access the application:

```bash
minikube service nginx-service
```

This opens the NGINX application in your browser.

---

# 📈 Kubernetes Scaling

The application initially runs with two replicas.

Check:

```bash
kubectl get deployment
```

Scale the application to five replicas:

```bash
kubectl scale deployment nginx-demo --replicas=5
```

Verify:

```bash
kubectl get pods
```

You should now see five NGINX Pods.

To continuously monitor the Pods:

```bash
kubectl get pods -w
```

---

# 🔥 Prometheus Setup

Prometheus and the Kubernetes monitoring components are installed using the **kube-prometheus-stack Helm chart**.

## Add Prometheus Repository

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
```

Update repositories:

```bash
helm repo update
```

Install the monitoring stack:

```bash
helm install monitoring prometheus-community/kube-prometheus-stack
```

Check the monitoring Pods:

```bash
kubectl get pods
```

You should see components such as:

* Prometheus
* Grafana
* Alertmanager
* Node Exporter
* kube-state-metrics

---

# 📊 Grafana Dashboard

Forward the Grafana service to your local machine:

```bash
kubectl port-forward svc/monitoring-grafana 3000:80
```

Open:

```text
http://localhost:3000
```

### 🔐 Grafana Credentials

Username:

```text
admin
```

Retrieve the generated password:

```bash
kubectl get secret monitoring-grafana \
-o jsonpath="{.data.admin-password}" | base64 --decode
```

Grafana can then be used to visualize:

* CPU utilization
* Memory utilization
* Pod status
* Node health
* Kubernetes resource usage
* Cluster metrics

---

# 🔎 Prometheus

Forward the Prometheus service:

```bash
kubectl port-forward svc/monitoring-kube-prometheus-prometheus 9090
```

Open:

```text
http://localhost:9090
```

Example Prometheus queries:

### CPU Metrics

```text
node_cpu_seconds_total
```

### Memory Metrics

```text
node_memory_MemAvailable_bytes
```

### Kubernetes Pod Information

```text
kube_pod_info
```

---

# 🧪 Commands Used

### Kubernetes

```bash
kubectl get nodes
kubectl get pods
kubectl get deployments
kubectl get services
kubectl describe pod <pod-name>
kubectl logs <pod-name>
```

### Scaling

```bash
kubectl scale deployment nginx-demo --replicas=5
kubectl get pods -w
```

### Minikube

```bash
minikube start --driver=docker
minikube status
minikube service nginx-service
```

### Helm

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
helm install monitoring prometheus-community/kube-prometheus-stack
```

---

# 🎯 Learning Outcomes

Through this project, I practiced:

* ☸️ Kubernetes application deployment
* 📦 Container orchestration
* 🔄 Replica management and scaling
* 🌐 Kubernetes Services
* 📈 Metrics collection with Prometheus
* 📊 Monitoring and visualization with Grafana
* ⎈ Helm-based Kubernetes deployments
* 🔍 Kubernetes observability fundamentals
* 🖥️ Cluster and Pod monitoring
* 🐳 Docker-based local Kubernetes environments

---

# 💡 Key Concepts Demonstrated

### Kubernetes Deployment

Manages the desired number of application Pods and ensures they remain available.

### Kubernetes Service

Provides network access to the application Pods and distributes traffic between replicas.

### Prometheus

Collects and stores time-series metrics from the Kubernetes environment.

### Grafana

Provides dashboards and visualizations for understanding system and application metrics.

### Helm

Simplifies the installation and management of complex Kubernetes applications such as the Prometheus monitoring stack.

---

# 🔮 Future Improvements

The project can be extended with:

* [ ] Custom application metrics
* [ ] Prometheus alerting rules
* [ ] Grafana custom dashboards
* [ ] Alertmanager notifications
* [ ] Horizontal Pod Autoscaler (HPA)
* [ ] Persistent storage for monitoring data
* [ ] Application-level monitoring
* [ ] CI/CD pipeline
* [ ] Deployment to AWS EKS
* [ ] Infrastructure provisioning using Terraform

---

# 👨‍💻 Author

**Pushkar Narayan**

Software Engineer
B.Tech — Computer Science & Information Technology

### Areas of Interest

`yaml` • `Pipeline` • `AWS` • `Docker` • `Kubernetes` • `Terraform` • `Linux` • `DevOps` • `Cloud` • `Monitoring`

---

## ⭐ Project Highlights

> **Kubernetes + Docker + Prometheus + Grafana**

A hands-on project demonstrating **container orchestration, Kubernetes scaling, metrics collection, and observability** using a complete local monitoring stack.

If you found this project useful, consider giving it a ⭐ on GitHub.
