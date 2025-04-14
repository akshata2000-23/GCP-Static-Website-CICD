---

```markdown
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

```
.
├── site-code/
│   ├── index.html
│   ├── assets/
│   └── ... (other static files)
└── .github/
    └── workflows/
        └── gcs-deploy.yml
```

---

## 🔧 Setup Instructions

### 1. Create a GCS Bucket

- Go to [Google Cloud Console](https://console.cloud.google.com/storage/browser)
- Click **"Create bucket"**
- Bucket name: Use a unique name (can match your domain name if using one)
- Enable **"Static website hosting"** in the bucket settings
- Upload your static files
- Make the bucket public:
  - Permissions > Add `allUsers` as **Storage Object Viewer**

---

### 2. Create Service Account & Upload Key to GitHub

- Go to **IAM & Admin → Service Accounts**
- Create a new service account with:
  - Role: **Storage Admin**
- Generate a **JSON key** and download it
- In your GitHub repo:
  - Go to **Settings → Secrets and variables → Actions**
  - Create a new **secret**
    - Name: `GCP_SA_KEY`
    - Paste the full contents of the JSON key

---

### 3. GitHub Actions Workflow

In your repo, create this file:  
`.github/workflows/gcs-deploy.yml`

```yaml
name: Deploy to GCS

on:
  push:
    branches:
      - master  # or main

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v3

      - name: Authenticate with Google Cloud
        uses: google-github-actions/auth@v1
        with:
          credentials_json: '${{ secrets.GCP_SA_KEY }}'

      - name: Set up gcloud CLI
        uses: google-github-actions/setup-gcloud@v1
        with:
          version: 'latest'

      - name: Upload files to GCS
        run: |
          gsutil -m cp -r site-code/* gs://your-gcs-bucket-name

      - name: List bucket contents
        run: |
          gsutil ls -r gs://your-gcs-bucket-name
```

> Replace `your-gcs-bucket-name` with the actual bucket name you created.

---

### 4. Trigger Deployment

- Make any code change (e.g., edit `index.html`)
- Push to the `master` (or main) branch
- GitHub Actions will automatically:
  - Authenticate with GCP
  - Upload the new files to your GCS bucket
  - Show logs including uploaded and listed files

---

## 🌍 Access Your Website

You can access your static website using:

```
https://storage.googleapis.com/YOUR_BUCKET_NAME/index.html
```

If you're using a custom domain (e.g., from No-IP or eu.org), you'll need:

- **GCP Load Balancer** with a **Static IP**  
- Point your domain's **A Record** to that IP  
- Configure backend bucket to point to your GCS bucket

---

## 📸 Demo Screenshot

_Add a screenshot of your site or pipeline running here_

---

## 📂 .gcloudignore (Optional)

If you're using the `gcloud` CLI, the `.gcloudignore` file specifies which files should **not** be uploaded to GCS. It works like `.gitignore`.

---

## 📝 Author

👩‍💻 **Akshata Shenoy**  
Made with ☁️ + ❤️ for learning Cloud & DevOps.

---
