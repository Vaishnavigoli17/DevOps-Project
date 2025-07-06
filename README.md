# 🛠️ DevOps To-Do App Deployment on AWS

This project demonstrates a complete DevOps pipeline and infrastructure setup for a full-stack To-Do application using industry best practices.

---

## 🚀 Tech Stack

- 🐳 Docker
- ☁️ AWS (EKS, ECR, EC2, IAM)
- ⚙️ Terraform
- ☸️ Kubernetes (EKS)
- 🔁 GitHub Actions (CI/CD)
- 📈 Prometheus + Grafana (Monitoring)

---

## 📦 Project Structure

```
.
├── backend/                 # Node.js API
├── frontend/                # React App
├── k8s/                     # Kubernetes manifests
├── .github/workflows/       # GitHub Actions CI/CD
├── terraform/               # Infrastructure as Code
└── README.md
```

---

## 🧱 Step 1 – Infrastructure Provisioning with Terraform

We use Terraform to create:
- VPC
- EKS Cluster
- EC2 Node Group
- IAM Roles
- ECR Repositories

### ✅ Commands

```bash
cd terraform
terraform init
terraform plan
terraform apply
```

To destroy everything:

```bash
terraform destroy
```

---

## 🐳 Step 2 – Dockerize Backend and Frontend

# User Screens

![](https://github.com/Vaishnavigoli17/DevOps-Project/blob/main/Screenshots/Register-Page.png?raw=true)
![](https://github.com/Vaishnavigoli17/DevOps-Project/blob/main/Screenshots/Login-Page.png?raw=true)
![](https://github.com/Vaishnavigoli17/DevOps-Project/blob/main/Screenshots/HomeTodo.PNG?raw=true)

# Run application (Local Testing)

- Pre-Requisite : Docker should be installed
- Follow steps below : 

        $ docker-compose up -d 
- Go to localhost:3000 to see the landing page
- The Node API Server will be running at localhost:5000/api/users
- After Testing : 

        $ docker-compose down

---

## ☁️ Step 3 – Push Docker Images to AWS ECR

### ✅ Commands

```bash
# Authenticate
aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin <your_account_id>.dkr.ecr.ap-south-1.amazonaws.com

# Tag & Push
docker tag todo-backend <ecr_url>/todo-backend:latest
docker push <ecr_url>/todo-backend:latest

docker tag todo-frontend <ecr_url>/todo-frontend:latest
docker push <ecr_url>/todo-frontend:latest
```

---

## ☸️ Step 4 – Kubernetes Deployment

### ✅ Commands

```bash
# Apply manifests
kubectl apply -f k8s/

# Verify
kubectl get pods
kubectl get svc
```

---

## 🔁 Step 5 – CI/CD with GitHub Actions

### ✅ CI Workflow

On push to `main`, GitHub Actions:
- Builds Docker images
- Pushes to ECR
- Deploys to EKS

### ✅ Secrets Required

| Name                   | Description                    |
|------------------------|--------------------------------|
| `AWS_ACCESS_KEY_ID`    | AWS IAM Access Key             |
| `AWS_SECRET_ACCESS_KEY`| AWS Secret Key                 |
| `KUBECONFIG_DATA`      | Base64 encoded kubeconfig      |
| `ECR_BACKEND_REPO`     | ECR repo URL for backend       |
| `ECR_FRONTEND_REPO`    | ECR repo URL for frontend      |

---

## 📈 Step 6 – Monitoring with Prometheus & Grafana

### ✅ Commands

```bash
# Add Helm repos
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update

# Install Prometheus
helm install prometheus prometheus-community/prometheus

# Install Grafana
helm install grafana grafana/grafana --set adminPassword=admin123

# Port forward to access Grafana
kubectl port-forward svc/grafana 3000:80
```

- Set Prometheus as the data source
- Add `$pod` variable to filter frontend/backend pods

---

## 🧹 Cleanup

```bash
terraform destroy
```

Also delete unused ECR images, EC2 volumes, and services from AWS Console.

---

## 👤 Author

**Vaishnavi Goli**  
DevOps Engineer  
☁️ Azure & GCP Certified | 📫 [LinkedIn](https://www.linkedin.com/in/vaishnavi-goli-519656205/)
