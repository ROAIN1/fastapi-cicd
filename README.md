---

# 🚀 FastAPI CI/CD Deployment using Docker & AWS EC2

This repository demonstrates the use of a CI/CD pipeline to automate the deployment of a simple FastAPI application using Docker and GitHub Actions, with deployment to an AWS EC2 instance.

The pipeline automatically builds and deploys the FastAPI app to an EC2 server whenever changes are pushed to the master branch of this repo.

## 📁 Repository Contents

- **main.py** – Your FastAPI application entry point.
- **requirements.txt** – Python dependencies needed by the app.
- **Dockerfile** – Configuration to containerize the FastAPI app.
- **.github/workflows/deploy.yml** – GitHub Actions workflow for CI/CD automation.

## ✅ Features

- FastAPI application running on Uvicorn.
- Docker containerization.
- GitHub Actions workflow to automate deployment.
- Deployment to AWS EC2 via SSH.
- PEM key handled securely using GitHub Secrets.

## 🌐 Technology Stack

- **Language & Framework:** Python, FastAPI
- **CI/CD:** GitHub Actions
- **Containerization:** Docker
- **Hosting/Deployment:** AWS EC2

## ⚙️ Prerequisites

Before using or deploying this project, ensure you have the following:

### On Your Local Machine:

- Python 3.8+
- Docker installed
- Git installed
- GitHub account

### On AWS:

- EC2 instance (Ubuntu or Amazon Linux recommended)
- Open ports 22 (SSH) and 8000 (FastAPI) in EC2 Security Group
- SSH access using a .pem key

## 🔐 GitHub Secrets Required

Set these secrets in your GitHub repository → Settings → Secrets → Actions:

| Secret Name       | Description                                      |
|-------------------|--------------------------------------------------|
| **EC2_HOST**      | Your EC2 public IP address (e.g., 54.123.45.67)  |
| **EC2_USER**      | EC2 SSH user (ubuntu for Ubuntu, ec2-user for Amazon Linux) |
| **EC2_SSH_KEY_B64** | Base64-encoded content of your .pem private key |

### 🔸 How to Convert PEM to Base64

```powershell
# On PowerShell (Windows)
[Convert]::ToBase64String([IO.File]::ReadAllBytes("C:\Path\To\EC2.pem")) > key.b64.txt
```

Copy the contents of `key.b64.txt` and paste it into the `EC2_SSH_KEY_B64` secret.

## 🧪 Run Locally

Clone the repository:

```bash
git clone https://github.com/ROAIN1/fastapi-cicd.git
cd fastapi-cicd
```

Set up Python virtual environment:

```bash
python -m venv venv
# Activate on Windows
venv\Scripts\activate
# Or on Linux/macOS
source venv/bin/activate
```

Install requirements:

```bash
pip install -r requirements.txt
```

Run the FastAPI app locally:

```bash
uvicorn main:app --reload
```

Open browser: http://127.0.0.1:8000
Docs: http://127.0.0.1:8000/docs

## 🐳 Run with Docker Locally (Optional)

```bash
docker build -t fastapi-app .
docker run -d -p 8000:8000 fastapi-app
```

## 🤖 CI/CD with GitHub Actions

This repository includes a CI/CD pipeline (`.github/workflows/deploy.yml`) that performs the following steps on every git push to master:

1. Checkout Code
2. Install Python & Docker
3. Decode PEM file from secret
4. SSH into EC2 instance
5. Transfer app files to EC2
6. Build & run the app using Docker on EC2

## 📂 Directory on EC2

Your app will be placed at `/home/ubuntu/fastapi-app` or `/home/ec2-user/fastapi-app` depending on the OS. Make sure that directory exists on EC2:

```bash
# Example for Ubuntu
mkdir -p /home/ubuntu/fastapi-app
```

## 🧪 Test API

Once deployed, visit:
`http://<your-ec2-ip>:8000/`

### API Docs:

- Swagger: `/docs`
- ReDoc: `/redoc`

## 🛠️ Troubleshooting

- **403 Error on Git Push:** Check if your GitHub token has correct access.
- **PEM Key Permission Denied:** Make sure the .pem file matches your EC2 instance.
- **Directory not found on EC2:** SSH into EC2 and manually create the target directory.
- **Port 8000 not accessible:** Ensure EC2 Security Group allows inbound traffic on port 8000.

---
