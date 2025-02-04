# 🚀 Node.js Todo App with CI/CD using GitHub Actions & ArgoCD

## 📌 Overview
This repository automates the CI/CD pipeline for the **Node.js Todo App** using **GitHub Actions, Docker, Kubernetes, and ArgoCD**. The workflow builds and pushes the application Docker image to Docker Hub, updates the ArgoCD repository, and triggers a deployment.

![alt text](<repo/tt.svg>)

## 🔧 Technologies Used
- **GitHub Actions** – Automates build and deployment.
- **Docker** – Containerizes the application.
- **Kubernetes** – Manages deployments and scaling.
- **ArgoCD** – Implements GitOps for continuous deployment.
- **MySQL** – Stores application data.
- **Microsoft Teams** – Sends deployment notifications.



## 🏗️ CI/CD Workflow
The GitHub Actions workflow consists of four jobs:

### 1️⃣ **Build & Push Docker Image**
- **Triggers** on `push` to the `main` branch.
- Builds a Docker image and tags it as `latest`.
- Retrieves the latest image tag from Docker Hub and increments it.
- Pushes the new image to Docker Hub.

### 2️⃣ **Generate Summary**
- Retrieves the newly created image tag.
- Generates a summary of the build process for GitHub Actions.

### 3️⃣ **Update ArgoCD Repository**
- Clones the `argocd-example-apps` repository.
- Updates the Kubernetes deployment manifest with the new image tag.
- Commits and pushes the changes.

### 4️⃣ **Notify Microsoft Teams**
- Sends a notification on **success** or **failure** of the workflow.

## 📜 Setup & Usage
### 1️⃣ **Pre-requisites**
Ensure you have the following configured:
- Docker Hub account with repository access.
- Kubernetes cluster managed by ArgoCD.
- A `k8s/deployment.yaml` file in the ArgoCD repository.
- A Microsoft Teams webhook URL for notifications.

### 2️⃣ **Secrets Configuration**
In your GitHub repository settings, add the following **secrets**:
| Secret Name          | Description |
|----------------------|-------------|
| `DOCKERHUB_TOKEN`   | Docker Hub access token |
| `GH_PAT`            | GitHub Personal Access Token (for pushing updates to ArgoCD repo) |
| `TEAMS_WEBHOOK_URL` | Microsoft Teams webhook URL |

### 3️⃣ **Deployment Steps**
1. **Push changes to `main` branch**
2. **GitHub Actions runs automatically**
3. **ArgoCD detects the new image tag and deploys it**
4. **Microsoft Teams receives a notification**

## 📂 Folder Structure
```
CD-REPO
📦 argocd
├── 📂 k8s                 # Kubernetes manifests
│   ├── deployment.yaml    # Kubernetes deployment definition
│   ├── service.yaml       # Kubernetes service definition

```
```
CI-REPO
📦 node-todo
├── 📂 .github
│ ├── 📂 workflows
│ │ ├── ci-cd.yml # GitHub Actions workflow for CI/CD
├── 📄 Dockerfile # Docker build file
├── 📄 README.md # Documentation
├── 📂 node_modules # Project dependencies
├── 📄 package.json # Project metadata and dependencies
├── 📂 repo # Repository-related files
├── 📂 spec # Test specifications
├── 📂 src # Source code
├── 📄 yarn.lock # Dependency lock file
```
## 🎯 ArgoCD Deployment File Example (`k8s/deployment.yaml`)
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: node-todo
spec:
  replicas: 2
  selector:
    matchLabels:
      app: node-todo
  template:
    metadata:
      labels:
        app: node-todo
    spec:
      containers:
        - name: node-todo
          image: algn48/node-todo:latest
          ports:
            - containerPort: 3000
```

## 📌 Monitoring & Troubleshooting
- **Check Workflow Runs:** GitHub Actions ➝ `Actions` Tab
- **Verify Image in Docker Hub:** [Docker Hub Repository](https://hub.docker.com/r/algn48/node-todo)
- **Monitor ArgoCD Deployment:** Run `kubectl get pods -n <namespace>`
- **Check Logs:** `kubectl logs -f <pod-name>`
- **Teams Notification:** Alerts for success/failure

## 📜 License
This project is licensed under the **MIT License**.

---

### 🔗 References
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Docker Hub](https://hub.docker.com)
- [ArgoCD Documentation](https://argo-cd.readthedocs.io/)

