
# 🧠 Medical RAG Chatbot

A Retrieval-Augmented Generation (RAG) based medical chatbot that ingests clinical PDFs and documents, embeds them using advanced NLP models, stores them in a vector store, and uses LLMs (via **Groq API**) to generate context-rich, medically-relevant answers.

---

## 📌 Features

- 🔎 Vector-based semantic search over medical PDFs
- 📄 PDF ingestion, chunking & embedding using LangChain + FAISS
- 🤖 Groq-powered LLM response generation
- 🖥️ Frontend UI with Flask + HTML/CSS
- 🚀 CI/CD pipeline: Jenkins + Docker + Trivy + AWS ECR + App Runner

---

## 🧱 Project Architecture

```

1️⃣ Project Setup
└── API Keys, Logging, Configuration

2️⃣ Data Layer
├── PDF Loader
├── Text Chunker
├── Embedding Generator (Groq)
└── Vector Store (FAISS)

3️⃣ Retrieval + LLM Layer
├── Groq Integration
└── Contextual Answering via RAG

4️⃣ Application Layer
├── Flask App (API & Routes)
└── Frontend UI

5️⃣ CI/CD & Deployment
├── Dockerize App
├── Jenkins Pipeline
└── AWS ECR + App Runner

````

---

## 🚀 Getting Started

### 📦 Clone the Repository

```bash
git clone https://github.com/Abhiram678/RAG_MEDICAL_CHATBOT.git
cd LLMOPS-2-TESTING-MEDICAL
````

### 🛠️ Create Virtual Environment (Windows)

```bash
python -m venv venv
rag\Scripts\activate
```

### 📥 Install Dependencies

```bash
pip install -e .
```

---

## ✅ Prerequisites Checklist

Before you begin, ensure you have:

* [ ] Docker Desktop installed and running
* [ ] This project pushed to a GitHub repo
* [ ] Dockerfile created for app
* [ ] Dockerfile created for Jenkins with Docker-in-Docker (DinD)

---

## 🧰 Jenkins Setup & Configuration

### 1️⃣ Build Jenkins Image

```bash
cd custom_jenkins
docker build -t jenkins-dind .
```

### 2️⃣ Run Jenkins Container

```bash
docker run -d ^
  --name jenkins-dind ^
  --privileged ^
  -p 8080:8080 ^
  -p 50000:50000 ^
  -v /var/run/docker.sock:/var/run/docker.sock ^
  -v jenkins_home:/var/jenkins_home ^
  jenkins-dind
```

### 3️⃣ Access Jenkins

* Visit: [http://localhost:8080](http://localhost:8080)
* To get admin password:

```bash
docker logs jenkins-dind
# Or manually:
docker exec jenkins-dind cat /var/jenkins_home/secrets/initialAdminPassword
```

### 4️⃣ Install Python in Jenkins Container

```bash
docker exec -u root -it jenkins-dind bash
apt update -y
apt install -y python3 python3-pip
ln -s /usr/bin/python3 /usr/bin/python
exit
```

### 5️⃣ Restart Jenkins

```bash
docker restart jenkins-dind
```

---

## 🔗 GitHub Integration with Jenkins

### 1️⃣ Generate GitHub Token

Go to GitHub → Settings → Developer Settings → PAT → Enable:

* `repo`
* `admin:repo_hook`

### 2️⃣ Add GitHub Token to Jenkins

* Jenkins Dashboard → Manage Jenkins → Credentials → (Global) → Add
* Fill in:

  * **Username:** GitHub username
  * **Password:** PAT token
  * **ID:** `github-token`

---

## ⚙️ Setup Pipeline in Jenkins

### 1️⃣ Create Pipeline Job

* Jenkins Dashboard → New Item → Pipeline
* Name: `medical-rag-pipeline`

### 2️⃣ Use Pipeline Syntax Generator

* Select: `checkout: General SCM`
* Set Git URL & credentials
* Copy generated script

### 3️⃣ Add `Jenkinsfile` to Project (Already Present)

If needed, create a `Jenkinsfile` in root and push:

```bash
git add Jenkinsfile
git commit -m "Add Jenkinsfile"
git push origin main
```

### 4️⃣ Trigger Pipeline

Go to Jenkins Dashboard → Select Job → Click **Build Now**

---

## 🐳 Docker + Trivy + AWS ECR Integration

### 1️⃣ Install Trivy in Jenkins Container

```bash
docker exec -u root -it jenkins-dind bash
curl -LO https://github.com/aquasecurity/trivy/releases/download/v0.62.1/trivy_0.62.1_Linux-64bit.deb
dpkg -i trivy_0.62.1_Linux-64bit.deb
trivy --version
exit
```

### 2️⃣ Create IAM User in AWS

* Assign programmatic access
* Attach policy: `AmazonEC2ContainerRegistryFullAccess`

### 3️⃣ Add AWS Credentials to Jenkins

* Jenkins → Manage Credentials → Add → AWS Credentials
* Add Access Key, Secret Key
* ID: `aws-ecr-creds`

---

### 4️⃣ Install AWS CLI in Jenkins

```bash
docker exec -u root -it jenkins-dind bash
apt install -y unzip curl
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
./aws/install
aws --version
exit
```

---

### 5️⃣ Create AWS ECR Repo

* Go to AWS Console → ECR → Create Repository
* Copy repository URI for later use

---

### 6️⃣ Fix Docker Daemon Permissions (If Needed)

```bash
docker exec -u root -it jenkins-dind bash
chown root:docker /var/run/docker.sock
chmod 660 /var/run/docker.sock
usermod -aG docker jenkins
exit
docker restart jenkins-dind
```

---

## ☁️ AWS App Runner Deployment

### ✅ IAM Permissions

* Attach `AWSAppRunnerFullAccess` to your IAM user

### 🔧 Manual Setup on AWS Console

1. Go to App Runner → Create Service
2. Source: Container registry (ECR)
3. Select your image and deploy
4. Configure runtime, env vars, etc.
5. Enable auto-deploy if needed

---

## 🔁 Full Jenkins Pipeline Flow

```
✅ Git Checkout
✅ Docker Build
✅ Trivy Security Scan
✅ Push to AWS ECR
✅ Deploy to AWS App Runner
```

In Jenkins: click **Build Now**
→ Your chatbot will be deployed live!

---

## 🔐 Security & Tips

* Store secrets in env vars or `.env` files
* You may restrict Trivy to fail on `--exit-code 1` for critical CVEs
* Use HTTPS + Auth for your deployed app
* Monitor Groq usage limits if applicable

---

## 🙋‍♂️ Contributing

Pull requests are welcome! For major changes, please open an issue first.

---

## 📜 License

Licensed under the MIT License.

---

## 🙌 Acknowledgments

Thanks to:

* [LangChain](https://github.com/hwchase17/langchain)
* [Groq](https://groq.com/)
* [FAISS](https://github.com/facebookresearch/faiss)
* [Trivy](https://github.com/aquasecurity/trivy)
* The Open Source ML Community ❤️

---


