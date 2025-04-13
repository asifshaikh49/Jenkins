
# Jenkins Essentials 🚀

## Introduction to CI Process 🌟

### What is Continuous Integration (CI)?
Continuous Integration (CI) ek software development practice hai jisme developers apne code ko frequently shared repository me integrate karte hain. Har integration ko automatically verify kiya jata hai:
- **Automated Build Processes 📦**
- **Automated Testing ✅**

### Benefits of CI:
- **🕒 Faster Feedback Loop**: Early issue detection.
- **🔒 Improved Code Quality**: Continuous testing and validation.
- **🤝 Collaboration**: Teams work cohesively.
- **📈 Efficiency**: Streamlined development workflow.

### Difference Between Continuous Delivery and Continuous Deployment:
| Aspect                   | Continuous Delivery 🚚                         | Continuous Deployment 🚀                     |
|--------------------------|-----------------------------------------------|---------------------------------------------|
| **Definition**            | Code is always deployable.                   | Every change is deployed automatically.     |
| **Deployment Trigger**    | Manual approval required.                    | Fully automated, no manual intervention.    |
| **Use Case**              | Suitable for environments requiring compliance. | Ideal for rapid release environments.      |
| **Automation Level**      | High, except production deployment.          | Fully automated, end-to-end.                |

---

## Introduction to Jenkins 🧰

### What is Jenkins?
Jenkins ek open-source automation server hai jo CI/CD pipelines me use hota hai. Yeh widely used hai for:
- **Building 🛠️**
- **Testing 🧪**
- **Deploying 🚀 applications**

### Why Jenkins?
- **🌍 Extensive Community Support**: Vast plugin ecosystem.
- **🖥️ Cross-Platform**: Runs on various OS.
- **📈 Scalable**: Small to large teams.
- **🔌 Integration**: Easily integrates with other tools.

---

## Installing Jenkins Server 🛠️

### Prerequisites:
- **Linux (Ubuntu/Debian)** OS.
- **Java 11+** installed.

### Installation Steps:
1. **Update System**:  
   `sudo apt update && sudo apt upgrade -y`
   
2. **Install Java**:  
   `sudo apt install openjdk-11-jdk -y`

3. **Add Jenkins Repository**:  
   ```bash
   curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee /usr/share/keyrings/jenkins-keyring.asc > /dev/null
   echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
   ```

4. **Install Jenkins**:  
   `sudo apt install jenkins -y`

5. **Start Jenkins Service**:  
   `sudo systemctl start jenkins`  
   `sudo systemctl enable jenkins`

6. **Access Jenkins Web Interface**:  
   Go to: `http://<your-server-ip>:8080`  
   Retrieve the password:  
   `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`

---

## Jenkins Essentials: Freestyle Job & Git Plugin 🎛️

### Create Your First Freestyle Job
1. Log in to Jenkins.
2. Click **New Item**, enter job name, and select **Freestyle project**.
3. Configure **Build Steps** (e.g., Execute shell).
4. Save and run.

---

## Jenkins Build Agents 🚀

### Install SSH Build Agent Plugin 🔌
- Navigate to **Manage Jenkins > Manage Plugins**.
- Install **SSH Build Agent Plugin**.

### Configure Build Agents:
1. Go to **Manage Jenkins > Manage Nodes**.
2. Add a new node, select **Permanent Agent**.
3. Configure using **Launch agent via SSH**.

---

## Jenkins Pipeline Essentials 📜

### Scripted vs Declarative Pipeline:
- **Scripted Pipeline**: Groovy-based, more flexible.
- **Declarative Pipeline**: Structured, easier for beginners.

### Example Jenkinsfile:
```groovy
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Building...'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing...'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying...'
            }
        }
    }
}
```

---

## Maven Essentials 🚀

### Installing Maven:
1. **Download Maven**: From the [Apache Maven site](https://maven.apache.org/).
2. **Set Environment Variables**:
   ```bash
   export M2_HOME=/opt/maven
   export PATH=$M2_HOME/bin:$PATH
   ```
3. **Verify Installation**:  
   `mvn -version`

### Creating a Maven Project:
```bash
mvn archetype:generate -DgroupId=com.example -DartifactId=my-app -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
```

---

## Troubleshooting Maven Builds in Jenkins

### Fix Jenkins File Permission Issues:
1. **Add Jenkins user to sudo group**:  
   `sudo usermod -aG sudo jenkins`
2. **Fix Ownership**:  
   `sudo chown -R jenkins:jenkins /var/lib/jenkins/`

---

# Conclusion 🌟
Jenkins CI/CD pipelines ko automate karta hai aur build, test, deployment ko efficiently manage karta hai. Yeh steps follow kar ke, aap easily Jenkins setup kar sakte hain aur apne projects ke liye fully automated pipelines create kar sakte hain.
