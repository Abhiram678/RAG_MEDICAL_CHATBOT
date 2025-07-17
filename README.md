Sure! Here's your **complete `README.md` file** with both the **project overview** and **CI/CD + Deployment** guide included.

---

```markdown
# 🧠 Medical RAG Chatbot

A Retrieval-Augmented Generation (RAG) based medical chatbot that ingests clinical PDFs and documents, embeds them using advanced NLP models, stores them in a vector store, and uses LLMs (via Groq API) to generate context-rich, medically-relevant answers.

---

## 📌 Features

- 🔎 Vector-based search over medical literature
- 📄 PDF ingestion and chunking using LangChain
- 💡 Context-aware response generation via Groq LLM
- 💬 Frontend UI with Flask & HTML/CSS
- 🚀 CI/CD pipeline with Jenkins, Trivy, Docker, AWS ECR, App Runner

---

## 🧱 Project Architecture

```

1️⃣ Project Setup & Config

* Project and API Setup
* Virtual Environment & Logging
* Exception Handling & Config Files

2️⃣ Data Processing

* Load & Chunk PDFs
* Generate Embeddings
* Store in FAISS Vector DB

3️⃣ LLM + Retrieval

* Setup Groq LLM
* Create Retriever Logic
* Use LLM to answer questions

4️⃣ App Layer

* Main Flask App
* Frontend: HTML + CSS

5️⃣ Deployment

* Dockerize App
* Jenkins CI/CD
* AWS ECR + App Runner

````

---

## 🚀 Getting Started

### 📦 Clone the Project

```bash
git clone https://github.com/data-guru0/LLMOPS-2-TESTING-MEDICAL.git
cd LLMOPS-2-TESTING-MEDICAL
````

### 🛠️ Set Up Virtual Environment (Windows)

```bash
python -m venv venv
venv\Scripts\activate
```

### 📥 Install Dependencies

```bash
pip install -e .
```

---

## ✅ Prerequisites Checklist

Ensure the following are completed before proceeding:

* [ ] Docker Desktop is installed and running
* [ ] GitHub repository is versioned and updated
* [ ] Dockerfile for application is ready
* [ ] Jenkins Dockerfile with Docker-in-Docker (DinD) is ready

---

## 🛠️ CI/CD Pipeline – Jenkins

### 1️⃣ Jenkins Setup

**a.** Create Jenkins Image:

```bash
cd custom_jenkins
docker build -t jenkins-dind .
```

**b.** Run Jenkins Container:

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

**c.** Get Admin Password:

```bash
docker logs jenkins-dind
```

Open Jenkins at: `http://localhost:8080`

---

### 2️⃣ GitHub Integration

**a.** Generate GitHub Token
Go to GitHub → Settings → Developer Settings → PAT → Enable `repo`, `admin:repo_hook`.

**b.** Add Credentials in Jenkins
Manage Jenkins → Credentials → Add:

* Username: your GitHub username
* Password: GitHub PAT
* ID: `github-token`

---

### 3️⃣ Jenkinsfile & Pipeline Setup

**a.** Add Jenkinsfile to your repo:

```bash
git add Jenkinsfile
git commit -m "Add Jenkinsfile"
git push origin main
```

**b.** In Jenkins:

* Create a new **Pipeline Job**
* Use the `github-token` to authenticate
* Use `Pipeline Script from SCM`

---

## 🐳 Docker + Trivy + ECR

### 1️⃣ Install Trivy (Vulnerability Scanner)

```bash
docker exec -u root -it jenkins-dind bash
curl -LO https://github.com/aquasecurity/trivy/releases/download/v0.62.1/trivy_0.62.1_Linux-64bit.deb
dpkg -i trivy_0.62.1_Linux-64bit.deb
exit
```

---

### 2️⃣ AWS Configuration for Jenkins

**a.** IAM User with Programmatic Access
Attach policy: `AmazonEC2ContainerRegistryFullAccess`

**b.** Add AWS Credentials in Jenkins:

* ID: `aws-ecr-creds`
* Access Key & Secret

---

### 3️⃣ Install AWS CLI in Jenkins Container

```bash
docker exec -u root -it jenkins-dind bash
apt update
apt install -y unzip curl
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
./aws/install
exit
```

---

### 4️⃣ Create ECR Repository in AWS

Go to AWS Console → ECR → Create Repository
Note the repository URI.

---

## 📦 Jenkinsfile Example Steps

Ensure your `Jenkinsfile` includes stages to:

* Build Docker image
* Scan with Trivy
* Push to ECR
* Deploy to AWS App Runner

---

## ☁️ Deploy to AWS App Runner

1. Go to AWS → App Runner → Create Service
2. Choose Source = ECR
3. Set configurations: CPU, Memory, ENV vars
4. Deploy and enable auto-deploy

---

## ✅ Final Step: Run Full Pipeline

In Jenkins:

```bash
Build Now
```

If successful, your Medical RAG Chatbot will be deployed and live via **AWS App Runner**.

---

## 👨‍💻 Developer Tips

* Keep secrets in environment variables or secret managers
* For better performance, consider GPU-based inference
* Use Groq for faster LLM calls

---

## 📜 License

MIT License

---

## 🙌 Acknowledgments

Thanks to LangChain, Groq, FAISS, Trivy, and the Open Source ML community ❤️

---

```

Let me know if you want:
- 🧪 A `README.dev.md` for local development only  
- 📛 Badges (build status, license, deploy live, etc.)  
- 📄 Sample Jenkinsfile or Dockerfile  
- 🌍 Hosting this on Hugging Face, Streamlit Cloud, or Vercel  
I'm happy to help polish every part!
```
