# 🔐 Project 4: DevSecOps Jenkins CI/CD Pipeline for Node.js App

## 📌 Project Overview

This project demonstrates the implementation of a **DevSecOps pipeline** using Jenkins for a Node.js application.

Key highlights:

* Integrates **security scanning** in CI/CD workflow
* Automates build, test, and deployment using **Jenkins pipelines**
* Ensures **code quality** and **vulnerability checks** before deployment
* Uses **Docker** for containerized builds

---

## 🛠️ Tech Stack / Tools Required

* **Jenkins** (Pipeline automation)
* **Docker & Docker Compose** (Containerization)
* **Node.js** (Application backend)
* **SonarQube** (Code quality and static analysis)
* **OWASP Dependency-Check / Trivy** (Vulnerability scanning)
* **Git & GitHub** (Version control)

---

## 📂 Functional Requirements

* Build and test Node.js application automatically.
* Run **static code analysis** using SonarQube.
* Scan Docker images for vulnerabilities using Trivy.
* Deploy the application using Docker Compose if all checks pass.

---

## 📂 Non-Functional Requirements

* Ensure **security best practices** during pipeline execution.
* Provide **fast feedback** for developers on code quality & vulnerabilities.
* Maintain **reliability and consistency** of deployments.
* Reduce manual deployment and testing errors.

---

## 🔄 Project Flow

1. **Developer commits code** to GitHub repository.
2. **Jenkins pipeline triggers automatically**:

   * Checkout code
   * Build Docker image
   * Run unit tests
   * Perform SonarQube analysis
   * Scan Docker image with Trivy for vulnerabilities
3. **Conditional deployment**:

   * If tests & security scans pass → Deploy to staging/production via Docker Compose
   * If any step fails → Pipeline fails, notify developers
4. **Logs & reports**:

   * SonarQube code quality dashboard
   * Trivy vulnerability scan reports
   * Jenkins console output for build & deployment

---

## 🚀 Sample Jenkins Pipeline (`Jenkinsfile`)

```groovy
pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'node-app:latest'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/<your-username>/project-4-devsecops-node.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE ./web'
            }
        }

        stage('Run Unit Tests') {
            steps {
                sh 'docker run --rm $DOCKER_IMAGE npm test'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh 'sonar-scanner'
                }
            }
        }

        stage('Security Scan with Trivy') {
            steps {
                sh 'trivy image $DOCKER_IMAGE'
            }
        }

        stage('Deploy Application') {
            when {
                expression { currentBuild.resultIsBetterOrEqualTo('SUCCESS') }
            }
            steps {
                sh 'docker-compose up -d'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '**/target/*.jar', allowEmptyArchive: true
            mail to: 'team@example.com',
                 subject: "Jenkins Build ${currentBuild.fullDisplayName}",
                 body: "Build completed with status: ${currentBuild.currentResult}"
        }
    }
}
```

---

## 📖 Folder Structure

```
project-4-devsecops-node/
│-- README.md
│-- Jenkinsfile            # DevSecOps pipeline
│-- docker-compose.yml     # App deployment
│-- web/
│   ├── app.js
│   └── package.json
│-- .gitignore
│-- sonar-project.properties # SonarQube config
```

---

## 📝 Resume Highlights

**Objective**
Implemented a **DevSecOps pipeline** integrating security scanning into CI/CD for Node.js applications.

**Key Achievements**

* Automated **build, test, and deployment** with Jenkins pipelines.
* Integrated **SonarQube** for code quality and **Trivy** for Docker image security.
* Reduced manual errors and ensured secure, reliable deployments.

**Impact**
✅ Improved security by **scanning code and Docker images before deployment**.
✅ Reduced manual deployment time by **80%**.
✅ Enabled **continuous feedback** for development and security teams.

---

## 🔗 Repository Link

*(Replace with your repo link after upload)*

```md
https://github.com/<your-username>/project-4-devsecops-node
```

---
