# CI/CD Pipeline Setup Guide - Section B

## Overview
This document provides complete instructions for Section B: CI/CD Automation using GitHub Actions.

## Pipeline Architecture

Our CI/CD pipeline consists of **5 stages**:

1. **Build & Test Backend** - Validates Node.js backend code
2. **Build & Test Frontend** - Builds React/Vite frontend
3. **Docker Build & Push** - Creates and pushes Docker images to Docker Hub
4. **Deploy to Kubernetes** - Deploys to Azure Kubernetes Service (AKS)
5. **Notification** - Reports pipeline status

---

## Prerequisites

### 1. GitHub Repository Setup
- Your code must be in a GitHub repository
- Repository: `muhammadowais87/Project-frontend`

### 2. Docker Hub Account
- Create account at: https://hub.docker.com
- Note your username for configuration

### 3. GitHub Secrets Configuration

Navigate to: **Repository → Settings → Secrets and variables → Actions**

Add the following secrets:

| Secret Name | Description | Example Value |
|------------|-------------|---------------|
| `DOCKER_USERNAME` | Your Docker Hub username | `muhammadowais87` |
| `DOCKER_PASSWORD` | Your Docker Hub password/token | `your-docker-password` |
| `AZURE_CREDENTIALS` | Azure service principal JSON | See Azure setup below |
| `AZURE_RESOURCE_GROUP` | Azure resource group name | `devops-lab-rg` |
| `AZURE_AKS_CLUSTER_NAME` | AKS cluster name | `devops-aks-cluster` |
| `VITE_API_URL` | Backend API URL | `http://backend-service:5000` |

---

## Azure Credentials Setup (For Kubernetes Deployment)

### Step 1: Install Azure CLI
```bash
# Windows (PowerShell)
winget install Microsoft.AzureCLI

# Or download from: https://aka.ms/installazurecliwindows
```

### Step 2: Login to Azure
```bash
az login
```

### Step 3: Create Service Principal
```bash
# Replace with your subscription ID
az ad sp create-for-rbac \
  --name "github-actions-devops" \
  --role contributor \
  --scopes /subscriptions/YOUR_SUBSCRIPTION_ID \
  --sdk-auth
```

### Step 4: Copy Output
The command will output JSON like this:
```json
{
  "clientId": "xxx",
  "clientSecret": "xxx",
  "subscriptionId": "xxx",
  "tenantId": "xxx",
  ...
}
```

**Copy this entire JSON** and add it as `AZURE_CREDENTIALS` secret in GitHub.

---

## Pipeline Trigger Configuration

The pipeline automatically runs on:

✅ **Push to branches:** `main`, `master`, `develop`
✅ **Pull requests to:** `main`, `master`, `develop`

### Manual Trigger
You can also trigger manually:
1. Go to **Actions** tab in GitHub
2. Select **CI/CD Pipeline - DevOps Lab Exam**
3. Click **Run workflow**

---

## Pipeline Stages Explained

### Stage 1: Build & Test Backend
```yaml
- Checkout code
- Setup Node.js 18
- Install dependencies (npm ci)
- Run ESLint linting
- Run automated tests
- Validate server.js syntax
- Upload artifacts
```

### Stage 2: Build & Test Frontend
```yaml
- Checkout code
- Setup Node.js 18
- Install dependencies
- Run ESLint linting
- Run tests
- Build production bundle (npm run build)
- Upload dist/ artifacts
```

### Stage 3: Docker Build & Push
```yaml
- Login to Docker Hub
- Build backend Docker image
- Build frontend Docker image
- Tag with branch name and SHA
- Push to Docker Hub registry
- Cache layers for faster builds
```

### Stage 4: Deploy to Kubernetes
```yaml
- Login to Azure
- Set AKS context
- Create namespace
- Apply Kubernetes manifests
- Wait for rollout completion
- Display service URLs
```

### Stage 5: Notification
```yaml
- Display status of all stages
- Show success/failure for each job
```

---

## Testing the Pipeline

### Method 1: Make a Code Change
```bash
# Make any small change
echo "# Pipeline test" >> README.md

# Commit and push
git add .
git commit -m "test: trigger CI/CD pipeline"
git push origin main
```

### Method 2: Create Pull Request
```bash
# Create new branch
git checkout -b feature/test-pipeline

# Make changes
echo "test" > test.txt

# Push and create PR
git add .
git commit -m "feat: test pipeline"
git push origin feature/test-pipeline
```

Then create PR on GitHub.

---

## Monitoring Pipeline Execution

### View Pipeline Run
1. Go to GitHub repository
2. Click **Actions** tab
3. Click on the latest workflow run
4. View each stage's logs

### Expected Results
- ✅ All 5 stages should show green checkmarks
- ✅ Docker images pushed to Docker Hub
- ✅ Kubernetes deployment successful (if AKS configured)

---

## Taking Screenshots for Submission

### Screenshot 1: Pipeline Overview
- Navigate to **Actions** tab
- Show the workflow run with all stages completed
- Capture: Workflow name, status, stages

### Screenshot 2: Stage Details
- Click on a workflow run
- Expand each stage to show logs
- Capture: All 5 stages with green checkmarks

### Screenshot 3: Docker Hub
- Login to Docker Hub
- Navigate to your repositories
- Show: `devops-backend` and `devops-frontend` images with tags

### Screenshot 4: Build Logs
- Click on "Build & Test Backend" stage
- Show the complete log output
- Capture: Dependencies installed, tests passed

### Screenshot 5: Docker Push Logs
- Click on "Build & Push Docker Images" stage
- Show successful push to registry
- Capture: Image digest and tags

---

## Troubleshooting

### Issue: Pipeline Fails at Docker Login
**Solution:** Verify `DOCKER_USERNAME` and `DOCKER_PASSWORD` secrets are correct

### Issue: Tests Fail
**Solution:** Run tests locally first:
```bash
cd backendsample
npm test
```

### Issue: Docker Build Fails
**Solution:** Test Docker build locally:
```bash
cd backendsample
docker build -t test-backend .
```

### Issue: Kubernetes Deployment Fails
**Solution:** 
- Verify Azure credentials are correct
- Ensure AKS cluster exists
- Check resource group name

---

## Local Testing (Before Push)

### Test Backend Build
```bash
cd backendsample
npm install
npm test
node -c src/server.js
```

### Test Frontend Build
```bash
cd frontendsample
npm install
npm run lint
npm run build
```

### Test Docker Images
```bash
# Backend
docker build -t devops-backend ./backendsample
docker run -p 5000:5000 devops-backend

# Frontend
docker build -t devops-frontend ./frontendsample
docker run -p 3000:80 devops-frontend
```

---

## Pipeline File Location

The pipeline configuration is located at:
```
.github/workflows/ci-cd-pipeline.yml
```

---

## Success Criteria for Section B

✅ Pipeline file created (Jenkinsfile/YAML)
✅ Build stage for frontend + backend
✅ Automated tests included
✅ Docker image build and push
✅ Deployment step configured
✅ Trigger on push/PR configured
✅ Screenshot showing all stages completed

---

## Next Steps

After completing Section B:
1. ✅ Take all required screenshots
2. ✅ Document in logsheet
3. ➡️ Proceed to Section C (Kubernetes/AKS deployment)
4. ➡️ Section D (Ansible configuration)
5. ➡️ Section E (Selenium testing)

---

## Support

If you encounter issues:
1. Check GitHub Actions logs
2. Verify all secrets are configured
3. Test Docker builds locally
4. Review error messages in pipeline logs

---

**Created for DevOps Lab Exam - Section B: CI/CD Automation**
