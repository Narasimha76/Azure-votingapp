
Objectives of the Project 📈:

1️⃣ Implementing Continuous Integration (CI):
1.Front-End Web App: Built in Python, allows voting between two options. 🗳️
2.Redis: Collects new votes. 🗃️
3. .NET Worker: Consumes votes and stores them. 🏗️
4.Postgres Database: Backed by a Docker volume. 💽
5.Node.js Web App: Displays voting results in real-time. 🌐

2️⃣ Continuous Integration Pipeline:
1.Created three pipelines for worker, vote, and result microservices. 🔄
2.Each pipeline builds a Docker image using Dockerfile from Azure Repo and pushes it to Azure Container Registry. 🐳
3.Shell Script: Writes to update the image name when new code is pushed to repository. This script ensures the new image is replaced, and ArgoCD deploys it in the AKS cluster. 📜🔄

3️⃣ Implementing Continuous Deployment (CD):
1.AKS Cluster: Created on Azure with Virtual Machine Scale Sets as node pools. ☁️
2.Deployment & Service YAMLs: Configured for the AKS cluster. 📜
3.ArgoCD Setup: Installed and configured to manage deployments. ⚙️

Azure DevOps | Python | Redis | .NET | Docker | AKS | ArgoCD | Postgres | Node.js 🧰💻
