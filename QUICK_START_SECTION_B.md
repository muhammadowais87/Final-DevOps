# Quick Start Guide - Section B CI/CD Setup

## 🚀 5-Minute Setup

### Step 1: Push Code to GitHub (if not already done)
```bash
cd d:\sampletest
git init
git add .
git commit -m "Initial commit with CI/CD pipeline"
git branch -M main
git remote add origin https://github.com/muhammadowais87/Project-frontend.git
git push -u origin main
```

---

### Step 2: Configure GitHub Secrets

1. Go to: https://github.com/muhammadowais87/Project-frontend/settings/secrets/actions

2. Click **"New repository secret"**

3. Add these secrets one by one:

#### Secret 1: DOCKER_USERNAME
- **Name:** `DOCKER_USERNAME`
- **Value:** `muhammadowais87` (your Docker Hub username)

#### Secret 2: DOCKER_PASSWORD
- **Name:** `DOCKER_PASSWORD`
- **Value:** Your Docker Hub password or access token
- **Get token:** https://hub.docker.com/settings/security → New Access Token

#### Secret 3: VITE_API_URL (Optional for now)
- **Name:** `VITE_API_URL`
- **Value:** `http://backend-service:5000`

---

### Step 3: Trigger the Pipeline

**Option A: Push a commit**
```bash
echo "# Test CI/CD" >> README.md
git add README.md
git commit -m "test: trigger CI/CD pipeline"
git push origin main
```

**Option B: Manual trigger**
1. Go to: https://github.com/muhammadowais87/Project-frontend/actions
2. Click "CI/CD Pipeline - DevOps Lab Exam"
3. Click "Run workflow" → "Run workflow"

---

### Step 4: Monitor Pipeline

1. Go to **Actions** tab: https://github.com/muhammadowais87/Project-frontend/actions
2. Click on the running workflow
3. Watch each stage complete:
   - ✅ Build & Test Backend (2-3 min)
   - ✅ Build & Test Frontend (3-4 min)
   - ✅ Docker Build & Push (5-7 min)
   - ⏭️ Deploy to Kubernetes (skip for now - needs Azure setup)
   - ✅ Notification

---

### Step 5: Verify Docker Images

1. Go to Docker Hub: https://hub.docker.com/repositories/muhammadowais87
2. You should see:
   - `devops-backend` with `latest` tag
   - `devops-frontend` with `latest` tag

---

### Step 6: Take Screenshots

#### Screenshot 1: Pipeline Overview
- URL: https://github.com/muhammadowais87/Project-frontend/actions
- Capture: Workflow list with green checkmark

#### Screenshot 2: Pipeline Stages
- Click on the workflow run
- Capture: All 5 stages with status

#### Screenshot 3: Build Logs
- Click "Build & Test Backend"
- Capture: Complete log output

#### Screenshot 4: Docker Push Success
- Click "Build & Push Docker Images"
- Capture: "Image pushed successfully" message

#### Screenshot 5: Docker Hub
- URL: https://hub.docker.com/repositories/muhammadowais87
- Capture: Both images listed

---

## ⚠️ Troubleshooting

### Pipeline fails at "Login to Docker Hub"
**Fix:** Check your Docker Hub credentials
```bash
# Test locally
docker login -u muhammadowais87
# Enter your password
```

### Pipeline fails at "Build Backend"
**Fix:** Test build locally
```bash
cd backendsample
npm install
npm test
```

### Pipeline fails at "Build Frontend"
**Fix:** Test build locally
```bash
cd frontendsample
npm install
npm run build
```

### "Deploy to Kubernetes" stage fails
**Expected:** This will fail until you set up Azure AKS (Section C)
**Action:** Ignore for now, focus on first 3 stages

---

## 📸 Screenshot Checklist for Section B

- [ ] GitHub Actions workflow list
- [ ] Pipeline run showing all stages
- [ ] Build & Test Backend logs
- [ ] Build & Test Frontend logs
- [ ] Docker Build & Push logs
- [ ] Docker Hub repositories page
- [ ] GitHub Secrets configuration page
- [ ] Pipeline YAML file content

---

## ✅ Success Criteria

Your pipeline is working correctly if:

1. ✅ All stages show green checkmarks (except K8s deploy if Azure not set up)
2. ✅ Docker images appear in Docker Hub
3. ✅ Build logs show "Tests passed"
4. ✅ No red X marks in pipeline

---

## 🎯 What to Submit for Section B

### Files to Submit:
1. `.github/workflows/ci-cd-pipeline.yml` - Your pipeline file
2. Screenshots folder with all required images
3. Logsheet with Section B completed

### Documentation to Include:
1. Pipeline stages explanation
2. Trigger configuration (push/PR)
3. Screenshot showing all stages completed
4. Docker Hub images proof

---

## 📝 For Your Logsheet - Section B

Copy this into your logsheet:

```
SECTION B: CI/CD AUTOMATION

Platform Used: GitHub Actions

Pipeline Stages:
1. Build & Test Backend - ✅ PASSED
2. Build & Test Frontend - ✅ PASSED  
3. Docker Build & Push - ✅ PASSED
4. Deploy to Kubernetes - ⏭️ CONFIGURED (pending Azure setup)
5. Notification - ✅ COMPLETED

Trigger Configuration:
- Automatic on push to: main, master, develop
- Automatic on pull requests to: main, master, develop
- Manual trigger available

Docker Images Created:
- muhammadowais87/devops-backend:latest
- muhammadowais87/devops-frontend:latest

Screenshots Attached:
- Pipeline overview
- All stages completed
- Build logs
- Docker Hub images
```

---

## 🔗 Useful Links

- **GitHub Actions Docs:** https://docs.github.com/en/actions
- **Docker Hub:** https://hub.docker.com
- **Your Repository:** https://github.com/muhammadowais87/Project-frontend
- **Actions Tab:** https://github.com/muhammadowais87/Project-frontend/actions

---

## ⏱️ Expected Timeline

- Setup secrets: 5 minutes
- First pipeline run: 15-20 minutes
- Taking screenshots: 10 minutes
- **Total: ~30-35 minutes**

---

## 🎓 Tips for Lab Exam

1. **Test locally first** - Don't rely only on pipeline
2. **Take screenshots immediately** - Don't wait
3. **Document everything** - More is better
4. **Keep logs** - Copy important log outputs
5. **Backup screenshots** - Save in multiple locations

---

**Ready to proceed?** Follow Step 1 and push your code! 🚀
