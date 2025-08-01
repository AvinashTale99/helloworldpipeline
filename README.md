
# 🚀 Python + Docker + Jenkins on AWS EC2

## 📘 Overview

This project demonstrates a simple CI/CD pipeline that:
- Uses a Python script (`helloworld.py`)
- Builds and runs the app in Docker
- Automates the process using Jenkins on an AWS EC2 instance

---

## ☁️ Prerequisites

Before getting started, ensure you have the following:

### 🔹 AWS EC2 Instance

- **AMI**: Amazon Linux 2023
- **Instance Type**: `t3.micro` or higher
- **Ports to Open in Security Group**:
  - **22** – SSH
  - **8080** – Jenkins
  - **80/443** – Optional for web traffic

### 🔹 SSH into EC2

```bash
ssh -i "your-key.pem" ec2-user@<your-ec2-ip>
```

---

## ⚙️ EC2 Instance Setup

### 📦 Install Required Software

```bash
sudo yum update -y
sudo yum install python3 git docker java-17-amazon-corretto -y

# Docker setup
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -aG docker ec2-user

# Jenkins setup
sudo curl --silent --location https://pkg.jenkins.io/redhat-stable/jenkins.io.key | sudo tee /etc/pki/rpm-gpg/jenkins.io.key
sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo yum install jenkins -y
sudo systemctl enable jenkins
sudo systemctl start jenkins
```

🔐 Reboot or re-login to apply Docker group changes.

Access Jenkins:  
**http://<your-ec2-ip>:8080**

---

## 📁 Project Structure

```bash
.
├── Dockerfile
├── Jenkinsfile
├── helloworld.py
└── README.md
```

---

## 🐳 Dockerfile

```dockerfile
FROM python:latest
WORKDIR /usr/app/src
COPY helloworld.py ./
CMD [ "python", "./helloworld.py"]
```

---

## 🤖 Jenkinsfile

```groovy
pipeline {
    agent any

    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/AvinashTale99/helloworldpipeline.git'
            }
        }

        stage('Run Script') {
            steps {
                sh 'python3 helloworld.py'
            }
        }
    }
}
```

---

## 🐍 Python Script

### `helloworld.py`

```python
print("Hello World !")
```

---

## 🧪 How to Use

### 🔹 Run Locally (Optional)

```bash
docker build -t helloworld .
docker run --rm helloworld
```

### 🔹 Run with Jenkins

1. Open Jenkins on `http://<your-ec2-ip>:8080`
2. Create a **Pipeline Project**
3. Use the `Jenkinsfile` from this repo
4. Run the pipeline and see the result!

---

## ✅ Permissions & Tips

- Make your `.pem` key secure:  
  ```bash
  chmod 400 your-key.pem
  ```
- If `docker` gives permission errors, re-login or reboot the EC2 instance.

---

## 📝 License

This project is licensed under the [MIT License](LICENSE).

---

## 👤 Author

**Avinash Tale**  
GitHub: [AvinashTale99](https://github.com/AvinashTale99)

---
