
# Jenkins CI/CD - Deep Dive Notes

This README contains in-depth notes and examples for mastering Jenkins and CI/CD pipelines.

---

## 1. Introduction to Jenkins & CI/CD

**Jenkins** is an open-source automation server used to automate the process of building, testing, and deploying software.

**CI (Continuous Integration):**
- Developers merge code frequently.
- Jenkins automatically builds and tests the code on every commit.

**CD (Continuous Delivery/Deployment):**
- Automatically delivers or deploys the code to production environments.

**CI/CD Workflow:**
```
Developer → Git Commit → Jenkins Job Trigger → Build → Test → Deploy → Notify
```

---

## 2. Jenkins Setup on VM

### Prerequisites:
- Java JDK 11+
- Ubuntu server (preferred)

### Installation Steps (Ubuntu):
```bash
sudo apt update
sudo apt install openjdk-11-jdk -y
wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb https://pkg.jenkins.io/debian binary/ > /etc/apt/sources.list.d/jenkins.list'
sudo apt update
sudo apt install jenkins -y
sudo systemctl start jenkins
sudo systemctl enable jenkins
```

Unlock Jenkins first-time password:
```bash
cat /var/lib/jenkins/secrets/initialAdminPassword
```

---

## 3. Jenkins UI / Dashboard / Jobs

**Key Sections:**
- New Item: Create jobs (Freestyle, Pipeline)
- Build History: Logs of past builds
- Manage Jenkins: Global configurations

**Job Config Tabs:**
- Source Code Management (SCM)
- Build Triggers
- Build Steps
- Post-build Actions

---

## 4. Jenkins Freestyle Project

A GUI-based job configuration used for simple tasks.

### Example: Deploy Java WAR to Tomcat
1. SCM: GitHub
2. Build Step:
```bash
mvn clean package
cp target/app.war /opt/tomcat/webapps/
```

---

## 5. Declarative Pipeline

Pipeline as code with structured syntax using `Jenkinsfile`.

### Example:
```groovy
pipeline {
  agent any
  stages {
    stage('Clone') { steps { git 'repo-url' } }
    stage('Build') { steps { sh 'mvn clean package' } }
    stage('Test') { steps { sh 'mvn test' } }
    stage('Deploy') { steps { sh 'scp target/*.war user@host:/webapps/' } }
  }
  post {
    success { echo 'Build succeeded!' }
    failure { echo 'Build failed!' }
  }
}
```

---

## 6. Jenkins Agents (Multi-Node)

Distributed build using master-agent model.

### Setup:
- Manage Jenkins → Nodes → New Node
- Use SSH credentials and labels
- In Jenkinsfile: `agent { label 'node-label' }`

---

## 7. Shared Libraries

Reusable pipeline logic across Jenkinsfiles.

### Structure:
```
vars/helloWorld.groovy
src/org/example/MyClass.groovy
```

### Usage:
```groovy
@Library('my-shared-library') _
helloWorld()
```

---

## 8. User Management in Jenkins (Role Based)

### Steps:
1. Install Role-Based Authorization Strategy Plugin
2. Create roles and assign permissions
3. Assign users to roles

---

## 9. CI/CD Project with Kubernetes, ArgoCD, Prometheus

### Flow:
1. Jenkins builds Docker image and pushes to DockerHub
2. Updates GitOps repo with Kubernetes YAML
3. ArgoCD detects changes and deploys to K8s
4. Prometheus monitors deployment

---

## 10. DevSecOps with OWASP, Trivy, SonarQube

| Tool       | Purpose                         |
|------------|----------------------------------|
| OWASP ZAP  | Web app security scan           |
| Trivy      | Container vulnerability scanner |
| SonarQube  | Code quality and bug detection  |

---

## 11. Email Notification on Pipeline Pass/Fail

### Setup:
- Install Email Extension plugin
- Configure SMTP settings

### Jenkinsfile Example:
```groovy
post {
  success {
    emailext(subject: "Build Success", body: "All good!", to: "team@example.com")
  }
  failure {
    emailext(subject: "Build Failed", body: "Please check Jenkins logs", to: "team@example.com")
  }
}
```

---

© Created by Asif Shaikh | DevOps Notes 2025
