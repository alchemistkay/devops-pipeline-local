# DevOps CI/CD Pipeline with Kubernetes and ArgoCD

## 🚀 Project Overview

This project implements a complete **DevOps CI/CD pipeline** in a **local environment** to showcase modern DevOps practices. It integrates various tools such as **Jenkins, SonarQube, Docker, Kubernetes, ArgoCD, and monitoring solutions** to automate software delivery, ensure code quality, and enhance observability.

## 🔧 Key Features

- **CI (Continuous Integration)**
  - Jenkins for build automation
  - SonarQube for code quality analysis
  - Docker & Docker Compose for containerization
  - DockerHub as a container registry
- **CD (Continuous Deployment)**
  - Kubernetes (Minikube) for local container orchestration
  - ArgoCD for GitOps-driven deployment
- **Monitoring & Logging**
  - Prometheus & Grafana for monitoring
  - Filebeat, OpenSearch & Kibana for centralized logging
  - All monitoring tools deployed via Helm charts

## 🏗 Architecture

```plaintext
+-----------------+        +------------------+         +-------------------+
| Developer Code  | -----> | Jenkins (CI/CD)  | ----->  | SonarQube (Code   |
| (GitHub)       |        | Pipeline         |         | Quality Analysis) |
+-----------------+        +------------------+         +-------------------+
            |                          |
            |                          v
            |                 +-----------------+
            |                 | DockerHub       |
            |                 | (Container Reg.)|
            |                 +-----------------+
            v                          |
  +----------------+                   v
  | Kubernetes     | <--------------- ArgoCD (GitOps)
  | (Minikube)    |
  +----------------+
            |
            v
+------------------------+
| Monitoring & Logging   |
| (Prometheus, Grafana,  |
| OpenSearch, Kibana)    |
+------------------------+
```

## 🛠️ Tech Stack

| Category          | Tools Used                                        |
| ----------------- | ------------------------------------------------- |
| Version Control   | Git, GitHub                                       |
| CI/CD Pipeline    | Jenkins, ArgoCD                                   |
| Code Quality      | SonarQube, Maven                                  |
| Containerization  | Docker, Docker Compose                            |
| Orchestration     | Kubernetes (Minikube)                             |
| Deployment        | Helm, ArgoCD                                      |
| Monitoring & Logs | Prometheus, Grafana, Filebeat, OpenSearch, Kibana |

## 📦 Prerequisites

Ensure you have the following installed on your local machine:

- Docker & Docker Compose
- Kubernetes (Minikube)
- Helm
- Git
- Jenkins
- SonarQube

## 🚀 Setup & Installation

### 1️⃣ Clone the Repository

```bash
git clone https://github.com/alchemistkay/devops-pipeline-local.git
cd devops-pipeline-local
```

### 2️⃣ Start Docker & Minikube

```bash
minikube start
```

### 3️⃣ Deploy Jenkins & SonarQube

```bash
docker compose up -d
```

### 4️⃣ Create namespaces for installing application, logging and monitoring

```bash
kubectl create ns app
kubectl create ns logging
kubectl create ns monitoring
```

### 5️⃣ Install Graphana and Promotheus in monitoring namespce using Helm

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm install prometheus prometheus-community/prometheus -n monitoring
kubectl port-forward service/prometheus-server 9030:80 -n monitoring
```

```bash
helm repo add grafana https://grafana.github.io/helm-charts
helm install grafana grafana/grafana -n monitoring
kubectl get secret --namespace monitoring grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
kubectl port-forward service/grafana 9040:80 -n monitoring
```

### 6️⃣ Install Elasticsearch, Kibana and Filebeat in logging namespace using Helm and port forward to access Elasticsearch and Kibana UIs

```bash
cd devops-demo-project/helm
helm install elasticsearch ./logging/elasticsearch
helm install kibana ./logging/kibana
helm install filebeat ./logging/filebeat

kubectl port-forward service/es-master-service 9010:9200 -n logging
kubectl port-forward service/kibana-service 9020:5601 -n logging
```

### 5️⃣ Install ArgoCD

```bash
cd devops-demo-project/cd/argocd
curl -sL https://github.com/operator-framework/operator-lifecycle-manager/releases/download/v0.28.0/install.sh | bash -s v0.28.0
kubectl get csv -n operators
kubectl create ns argocd
kubectl apply -f argocd-basic.yaml
echo $(kubectl get secret example-argocd-cluster -n argocd -o jsonpath="{.data.admin\.password}" | base64 —decode)
minikube service example-argocd-server -n argocd
```

## 📊 Monitoring & Logging

- Access **Prometheus**: `http://localhost:9030`
- Access **Grafana**: `http://localhost:9040`
- Access **Kibana**: `http://localhost:9020`

## 📜 License

This project is open-source and available under the **MIT License**.

## 🤝 Contributing

Feel free to submit pull requests or open issues to improve this project!

## 📩 Contact

For queries, reach out via [**LinkedIn**](https://www.linkedin.com/in/kwaku-danso-366196120/) or [**email@example.com**](mailto\:kwaku.danso03@gmail.com).

