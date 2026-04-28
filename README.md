# 🌍 Full-Stack DevOps Project — Terraform + Docker + Frontend + Go

> An end-to-end DevOps project demonstrating **Infrastructure as Code (IaC)** with Terraform, a containerized frontend, a Go-based backend, and an automated CI/CD pipeline via GitHub Actions — all wired together from a single repository.

---

## 📌 Project Overview

This project demonstrates a **complete DevOps workflow** where infrastructure is provisioned, application code is containerized, and deployments are automated — following real-world engineering practices.

**What makes this a DevOps project:**
- Infrastructure provisioned as code using **Terraform (HCL)** — no manual cloud console clicks
- Frontend application containerized with **Docker**, served via Nginx
- **Go** used for backend or tooling logic
- **GitHub Actions** CI/CD pipeline for automated build and deploy
- Clean monorepo structure separating application code from infrastructure

---

## 🏗️ Architecture Overview

```
┌────────────────────────────────────────────────────────┐
│                     GitHub Repository                   │
│                                                         │
│   ┌──────────────┐        ┌──────────────────────────┐ │
│   │   frontend/   │        │      terraform/           │ │
│   │  React/JS App │        │  Infrastructure as Code  │ │
│   │  + Dockerfile │        │  (Providers, Resources,  │ │
│   └──────┬───────┘        │   Variables, Outputs)    │ │
│          │                 └──────────────┬───────────┘ │
└──────────┼──────────────────────────────┼─────────────┘
           │                              │
           ▼                              ▼
┌──────────────────┐           ┌───────────────────────┐
│  GitHub Actions  │           │   Cloud Provider       │
│  CI/CD Pipeline  │──────────▶│   (AWS / GCP / Azure)  │
│                  │  deploys  │   Provisioned by TF    │
└──────────────────┘           └───────────────────────┘
           │
           ▼
┌──────────────────┐
│  Docker Registry  │
│  (Built Image)   │
└──────────────────┘
```

---

## 🛠️ Tech Stack

| Layer | Technology | Purpose |
|---|---|---|
| **Infrastructure** | Terraform (HCL) | Provision and manage cloud resources as code |
| **Frontend** | JavaScript + CSS + HTML | Web application UI |
| **Backend / Tooling** | Go (Golang) | Backend service or infrastructure tooling |
| **Containerization** | Docker | Package and run the application consistently |
| **CI/CD** | GitHub Actions | Automated pipeline: build, test, deploy |
| **Web Server** | Nginx | Serve the frontend static build in production |

> **Language breakdown:** HCL 64% · CSS 18% · JavaScript 14% · HTML 2% · Go 1.1% · Dockerfile 0.7%

---

## 📂 Project Structure

```
project1/
│
├── frontend/                  # Frontend web application
│   ├── src/                   # JS/CSS/HTML source files
│   ├── public/                # Static assets
│   ├── Dockerfile             # Container build for the frontend
│   └── package.json           # Frontend dependencies & scripts
│
├── terraform/                 # Infrastructure as Code
│   ├── main.tf                # Core resource definitions
│   ├── variables.tf           # Input variable declarations
│   ├── outputs.tf             # Output values (IPs, URLs, etc.)
│   ├── provider.tf            # Cloud provider configuration
│   └── terraform.tfvars       # Variable values (not committed)
│
└── .github/
    └── workflows/
        └── *.yml              # GitHub Actions CI/CD pipeline
```

---

## 🟦 Terraform — Infrastructure as Code

The `terraform/` directory contains all cloud infrastructure definitions. This means the entire environment — networking, compute, storage, DNS — can be **created, modified, and destroyed** with a single command. No manual steps.

### Key Concepts Used

| Concept | Description |
|---|---|
| **Providers** | Declare which cloud (AWS/GCP/Azure) Terraform manages |
| **Resources** | Define actual infrastructure (VMs, VPCs, S3 buckets, etc.) |
| **Variables** | Parameterise configs for reuse across environments |
| **Outputs** | Expose values (like public IPs or DNS) after apply |
| **State** | Terraform tracks real-world resource state in `terraform.tfstate` |

### Terraform Workflow

```bash
# 1. Initialise — download provider plugins
terraform init

# 2. Plan — preview what will be created/changed/destroyed
terraform plan

# 3. Apply — provision the infrastructure
terraform apply

# 4. Destroy — tear down all resources when done
terraform destroy
```

> ⚠️ **Never commit `terraform.tfvars` or `.tfstate` files** — they may contain secrets and environment-specific values.

---

## 🐳 Docker — Frontend Container

The `frontend/Dockerfile` packages the web application into a portable container image.

```bash
# Build the image
docker build -t project1-frontend ./frontend

# Run locally
docker run -d -p 80:80 project1-frontend

# Open in browser
open http://localhost
```

---

## 🔄 CI/CD Pipeline — GitHub Actions

The `.github/workflows/` pipeline runs automatically on every push to `main`:

```
┌──────────┐    ┌──────────┐    ┌─────────────┐    ┌──────────────────┐
│ Checkout  │───>│  Install  │───>│    Build    │───>│  Docker Build    │
│   Code    │    │  & Lint   │    │  Frontend   │    │    & Push        │
└──────────┘    └──────────┘    └─────────────┘    └────────┬─────────┘
                                                             │
                                              ┌──────────────▼──────────┐
                                              │  terraform apply        │
                                              │  (Provision / Update    │
                                              │   Infrastructure)       │
                                              └─────────────────────────┘
```

**Pipeline stages:**
1. **Checkout** — Pull the latest source code
2. **Install & Lint** — Install frontend dependencies, run linting
3. **Build** — Compile and bundle the frontend application
4. **Docker Build & Push** — Build container image and push to registry
5. **Terraform Apply** — Provision or update cloud infrastructure

---

## 💻 Local Development

### Prerequisites

| Tool | Version | Install |
|---|---|---|
| Node.js | ≥ 18 | [nodejs.org](https://nodejs.org) |
| Docker | ≥ 20 | [docker.com](https://docker.com) |
| Terraform | ≥ 1.0 | [terraform.io](https://developer.hashicorp.com/terraform/install) |
| Go | ≥ 1.21 | [go.dev](https://go.dev) |

### Frontend Setup

```bash
cd frontend

# Install dependencies
npm install

# Start dev server
npm run dev
# → http://localhost:5173 (or 3000)

# Production build
npm run build
```

### Terraform Setup

```bash
cd terraform

# Create your variable values file (never commit this)
cp terraform.tfvars.example terraform.tfvars
# Edit terraform.tfvars with your cloud credentials/config

# Initialise providers
terraform init

# Preview changes
terraform plan

# Apply
terraform apply
```

### Go (Backend / Tooling)

```bash
# Run Go code
go run .

# Build binary
go build -o app .
```

---

## 🔑 Key DevOps Concepts Demonstrated

**1. Infrastructure as Code (IaC)**
All cloud resources are defined in version-controlled HCL files. The infrastructure is reproducible, reviewable in PRs, and consistent across environments — eliminating "it works on my cloud" problems.

**2. Immutable Infrastructure**
Docker images are built fresh on every pipeline run and tagged with the commit SHA. Deployments replace containers entirely rather than mutating running servers.

**3. Separation of Concerns**
The repository is cleanly split: `frontend/` owns the application, `terraform/` owns the infrastructure. Each can evolve independently.

**4. Pipeline as Code**
The CI/CD pipeline is defined in YAML, committed to the repo, and version-controlled alongside the application and infrastructure. No manual deployment steps exist.

**5. Least-Privilege Security**
Cloud credentials are stored as **GitHub Secrets** — never hardcoded. Terraform variables keep sensitive config out of source control.

**6. Cloud-Native Thinking**
The project follows cloud-native principles: stateless containers, declarative infrastructure, and automation-first deployment.

---

## 🔒 Security Best Practices Followed

- Cloud credentials stored in **GitHub Actions Secrets** — never in code
- `terraform.tfvars` and `.tfstate` excluded via `.gitignore`
- Docker images built from minimal Alpine base images
- No hardcoded IPs, passwords, or API keys in source files

---

## 📋 Prerequisites Summary

| Tool | Purpose |
|---|---|
| Node.js ≥ 18 | Frontend development |
| Docker ≥ 20 | Container builds and local testing |
| Terraform ≥ 1.0 | Infrastructure provisioning |
| Go ≥ 1.21 | Backend / tooling compilation |
| Cloud CLI (aws/gcloud/az) | Authenticate Terraform with your cloud provider |

---

## 👤 Author

**Anand** — [GitHub @Anand-1110](https://github.com/Anand-1110)

---

> ⭐ If you found this project useful, feel free to star the repository!
