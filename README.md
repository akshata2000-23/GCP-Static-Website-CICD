# 🌐 GCP Static Website with CI/CD using GitHub Actions

This project demonstrates how to host a **static website** on **Google Cloud Storage (GCS)** and set up a **CI/CD pipeline using GitHub Actions**. Every time a code change is pushed to the repo, it automatically updates the website content in the GCS bucket.

---

## 🚀 Features

- 📁 Host static files on GCS bucket  
- 🔐 Service Account authentication with GitHub  
- 🔄 Automatic deployment via GitHub Actions  
- 🌍 Custom domain mapping (Optional: with No-IP or other providers)  
- ✅ Public bucket configuration for static site access  
---
## 🧱 Folder Structure

---

## 🔧 Setup Instructions

### 1. Create a GCS Bucket
- Go to Google Cloud Console → Storage → Create a bucket.
- Name it something like `your-site-bucket-name`.
- Enable **static website hosting**.
- Make it **public** (set `allUsers` to have **Storage Object Viewer** role).

### 2. Service Account & Key
- Create a **Service Account** in GCP with permission: `Storage Admin`.
- Download the **JSON key**.
- Go to your GitHub repo → Settings → Secrets → Add secret:
  - Name: `GCP_SA_KEY`
  - Value: Paste the JSON contents

### 3. GitHub Actions Pipeline

The GitHub Actions workflow file:  
📄 `.github/workflows/gcs-deploy.yml`
