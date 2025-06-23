
# 🚀 Jenkins CI/CD - Ultimate Deep Dive Notes by Asif Shaikh

Welcome to the Jenkins & CI/CD learning guide.  
These notes are perfect for interviews, real-world DevOps projects, and mastering pipelines like a pro. 💡🛠️

---
## 🧠 1. Jenkins & CI/CD 

### ✅ What is Jenkins?

- Jenkins is an **automation server** that automates your **Software Development Lifecycle (SDLC)**.
- Written in Java, it follows a **plugin-based architecture**.
- Jenkins jobs can automatically **build, test, package, and deploy** source code.
- Its biggest power lies in **Pipeline as Code** via `Jenkinsfile`.

---

### 🔥 Real-Life CI/CD Example Using Jenkins

1. Developer pushes code to GitHub.
2. GitHub webhook notifies Jenkins.
3. Jenkins performs the following:
   - Pulls the latest code
   - Builds it using Maven or Gradle
   - Runs unit tests
   - Deploys the app (e.g., Tomcat, Docker, etc.)
   - Sends status emails to the team

---

### 📊 CI vs CD (Delivery) vs CD (Deployment)

| Aspect       | CI (Continuous Integration) | CD (Continuous Delivery)         | CD (Continuous Deployment)     |
| ------------ | --------------------------- | -------------------------------- | ------------------------------- |
| Purpose      | Integrate code frequently   | Deliver code to staging          | Deploy code to production       |
| Jenkins Role | Build + Unit Test           | Build + Test + Approval           | Auto-build + Auto-deploy        |
| Example      | `mvn test`                  | `scp WAR file to staging`        | `helm install on prod cluster`  |
| Risk Level   | Low                         | Medium                            | High                            |
| Tool Chain   | Git, Maven, JUnit           | Tomcat, SCP, Ansible              | Kubernetes, Docker, ArgoCD      |

---

### 💬 Interview Questions:

- What’s the difference between CI and CD?
- How does Jenkins support the full CI/CD process?
- What is Jenkins' default port, and where can you configure it?

---

### 🧪 Hands-on Task:

```bash
# Create a sample Java project with JUnit
# Push to a GitHub repository
# Create a Freestyle Job in Jenkins
# Configure it to Poll SCM every minute: */1 * * * *
# Observe automatic build pipeline in action


---

**### 🔐 First Time Unloc** 🖥️

### ✅ Requirements:
- Java JDK 11+ ☕
- Linux (Ubuntu/Debian/CentOS) preferred 🐧
- Root/Sudo Access 🔐

### 🔧 Installation Steps (Ubuntu):
```bash
# Install Java ☕
sudo apt update
sudo apt install openjdk-11-jdk -y

# Add Jenkins Repo 📦
wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb https://pkg.jenkins.io/debian binary/ > /etc/apt/sources.list.d/jenkins.list'

# Install Jenkins 🛠️
sudo apt update
sudo apt install jenkins -y

# Start and Enable Jenkins Service 🔁
sudo systemctl start jenkins
sudo systemctl enable jenkins

# Access URL 🌐: http://<ip>:8080
```

### 🔐 First Time Unlock:
```bash
cat /var/lib/jenkins/secrets/initialAdminPassword
```

---

## 🖥️ 3. Jenkins UI / Dashboard / Job Types

### 📊 Jenkins Dashboard:
- 🔄 View build history
- ➕ Create new jobs
- 🔧 Manage Jenkins (plugins, credentials, nodes)

### 🧱 Job Types:
- ⚙️ Freestyle Project
- 🧪 Pipeline
- 🧵 Multi-branch pipeline
- 🗂️ Folder and GitHub Organization

### 🔧 Job Configuration Tabs:
- SCM (GitHub, Bitbucket)
- Build Triggers (Webhook, Poll SCM)
- Build Steps (Shell, Gradle, Maven)
- Post-Build Actions (Email, Archive, Deploy)

---

## 🛠️ 4. Jenkins Freestyle Project

### 🔹 What is it?
- GUI-driven basic job configuration
- Good for small tasks and beginners

### 🧪 Sample Use-Case:
```bash
mvn clean package
cp target/myapp.war /opt/tomcat/webapps/
```

### 📌 Limitations:
- ❌ Not code-based (no Jenkinsfile)
- ❌ Harder to version/control

---

## 🧾 5. Declarative Pipelines (Jenkinsfile)

### 🔹 What is a Pipeline?
- CI/CD as **code** 🤖
- Stored in repo with your application code

### 🧰 Sample Declarative Pipeline:
```groovy
pipeline {
  agent any
  environment {
    JAVA_HOME = '/usr/lib/jvm/java-11-openjdk'
  }
  stages {
    stage('Clone') {
      steps { git 'https://github.com/asif/myapp.git' }
    }
    stage('Build') {
      steps { sh 'mvn clean install' }
    }
    stage('Test') {
      steps { sh 'mvn test' }
    }
    stage('Deploy') {
      steps { sh 'scp target/*.war user@server:/opt/tomcat/webapps/' }
    }
  }
  post {
    success { echo '✅ Build Passed!' }
    failure { echo '❌ Build Failed!' }
  }
}
```

---

## 🌐 6. Jenkins Agents (Multi-Node Build Farm)

### 🔹 Why Use Agents?
- Run parallel jobs 🧵
- Distribute load 💡
- Isolate OS-specific jobs 🖥️

### 🔧 Setup:
1. Go to **Manage Jenkins → Manage Nodes** 🧱
2. Add new node (SSH, Docker, etc.) 🔗
3. Assign labels → use in `Jenkinsfile`

```groovy
agent { label 'linux-docker-agent' }
```

---

## 📦 7. Shared Libraries

### 🔹 Why?
- DRY principle (Don't Repeat Yourself) ✅
- Share logic across teams 🔁

### 🗃️ Structure:
```
vars/helloWorld.groovy
src/org/company/utils.groovy
```

### 🔧 Jenkinsfile:
```groovy
@Library('shared-lib') _
helloWorld()
```

---

## 🧑‍💼 8. User Management (RBAC)

### 🔹 Plugin:
- 🛡️ Role-Based Authorization Strategy

### 👥 Steps:
1. Install plugin → Configure Global Security 🔐
2. Create roles (admin, dev, viewer)
3. Assign users → Roles

### Example:
- Dev can trigger builds 🔁
- Viewers see logs only 👁️

---

## ☸️ 9. CI/CD with Kubernetes, ArgoCD, Prometheus

### 🔹 Flow:
```text
Git Push → Jenkins → Docker Build → Push Image → GitOps Repo Update → ArgoCD → Deploy to K8s → Monitor via Prometheus 📈
```

### 📦 Technologies:
- Jenkins 🧪
- Docker 🐳
- Kubernetes ☸️
- ArgoCD 🔄
- Prometheus 📊

---

## 🔐 10. DevSecOps with Trivy, SonarQube, OWASP

| 🔧 Tool | 🔍 Purpose |
|--------|-----------|
| Trivy  | Scan Docker images for vulnerabilities 🐞 |
| SonarQube | Code quality, bugs, coverage 📊 |
| OWASP ZAP | Dynamic app security scan 🔐 |

### 🔧 Jenkinsfile Integration:
```groovy
sh 'trivy image myimage:latest'
withSonarQubeEnv('SonarQube') {
  sh 'mvn sonar:sonar'
}
```

---

## 📧 11. Email Notifications

### 📩 Plugin:
- Email Extension Plugin

### 🔧 Configure SMTP:
- Gmail, SendGrid, SES etc.

### 📜 Jenkinsfile:
```groovy
post {
  success {
    emailext(subject: "✅ Build Success", body: "Build Passed!", to: "team@example.com")
  }
  failure {
    emailext(subject: "❌ Build Failed", body: "Check Jenkins Logs", to: "team@example.com")
  }
}
```

---

📘 **Made with ❤️ by Asif Shaikh - DevOps Engineer (2025)**  
🔗 GitHub: [asifshaikh-dev](https://github.com/asifshaikh49)

