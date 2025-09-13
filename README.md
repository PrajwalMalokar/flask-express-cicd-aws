# Flask + Express CI/CD Deployment on AWS EC2 ✅

## Project Overview 🚀

This project demonstrates the deployment of a **Flask backend** and an **Express frontend** on a single **AWS EC2 instance**, along with a **CI/CD pipeline using Jenkins** to automate deployments.

---

## Architecture 🏗️

* **EC2 Instance**: Hosts both Flask and Express applications. ✅
* **Flask Backend**: Runs on port `5000`. ✅
* **Express Frontend**: Runs on port `3000`. ✅
* **PM2**: Ensures applications remain running. ✅
* **Jenkins**: Automates deployment on every Git push. 🔄

---

## Setup 🛠️

### EC2 Instance

1. Launch an EC2 instance (free-tier eligible recommended).
2. SSH into the instance and install:

   * Python (for Flask) ✅
   * Node.js (for Express) ✅
   * Git (to pull the repositories) ✅

---

### Application Deployment 📦

#### Flask Backend

1. Clone the repository:

```bash
git clone https://github.com/PrajwalMalokar/flask-express-cicd-aws.git
cd flask-express-cicd-aws/backend
```

2. Install dependencies:

```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt ✅
```

3. Start the application using PM2:

```bash
pm2 start app.py --interpreter=python3 --name flask-app --update-env ✅
```

#### Express Frontend

1. Clone the repository:

```bash
git clone https://github.com/PrajwalMalokar/flask-express-cicd-aws.git
cd flask-express-cicd-aws/frontend
```

2. Install dependencies:

```bash
npm install ✅
```

3. Start the application using PM2:

```bash
pm2 start app.js --name express-app --update-env ✅
```

---

## CI/CD with Jenkins 🤖

* **Jenkins Installation**: Installed on the EC2 instance or a separate server. ✅
* **Pipelines**: Two separate pipelines for Flask and Express. ✅

**Pipeline Steps:**

1. Pull the latest code from GitHub 🔄
2. Install dependencies (`pip install` for Flask, `npm install` for Express) ✅
3. Restart applications using PM2 ✅
4. Trigger pipelines automatically using **GitHub webhooks** on every push 🔔

**Jenkinsfile Example (Flask):**

```groovy
pipeline {
    agent any
    stages {
        stage('Deploy Flask') {
            steps {
                sh "pm2 stop flask-app || true"
                sh "rsync -av --delete $WORKSPACE/backend/ /home/ubuntu/flask-express-cicd-aws/backend/"
                sh '''
                    cd /home/ubuntu/flask-express-cicd-aws/backend
                    source venv/bin/activate
                    pip install -r requirements.txt
                    pm2 start app.py --interpreter=python3 --name flask-app --update-env
                '''
            }
        }
    }
}
```

**Jenkinsfile Example (Express):**

```groovy
pipeline {
    agent any
    stages {
        stage('Deploy Express') {
            steps {
                sh "pm2 stop express-app || true"
                sh "rsync -av --delete $WORKSPACE/frontend/ /home/ubuntu/flask-express-cicd-aws/frontend/"
                sh '''
                    cd /home/ubuntu/flask-express-cicd-aws/frontend
                    npm install
                    pm2 restart express-app || pm2 start app.js --name express-app --update-env
                '''
            }
        }
    }
}
```

---

## Running the Project 🌐

* Access the Flask backend: `http://<EC2_PUBLIC_IP>:5000` ✅
* Access the Express frontend: `http://<EC2_PUBLIC_IP>:3000` ✅

All updates pushed to GitHub are automatically deployed through Jenkins 🔄

---

## Notes 📝

* PM2 ensures both applications stay alive after server restarts ✅
* Jenkins automates deployment, reducing manual effort and improving consistency ✅
* Both applications are kept in sync with the Git repositories via CI/CD pipelines 🔄
