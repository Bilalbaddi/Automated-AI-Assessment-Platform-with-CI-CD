# 🤖 Automated AI Assessment Platform (LLMOps & GitOps)



[![Build Status](https://img.shields.io/jenkins/build?jobUrl=http%3A%2F%2FYOUR_JENKINS_URL%2Fjob%2Fstudy%20ai&label=Jenkins%20Build&logo=jenkins)](http://YOUR_JENKINS_URL/job/study%20ai/)

![Docker Pulls](https://img.shields.io/docker/pulls/bilal1045/studyai?logo=docker)

![Python](https://img.shields.io/badge/Python-3.10-blue?logo=python&logoColor=white)

![Kubernetes](https://img.shields.io/badge/Kubernetes-Minikube-blue?logo=kubernetes)

![ArgoCD](https://img.shields.io/badge/GitOps-ArgoCD-orange?logo=argo)

![LangChain](https://img.shields.io/badge/AI-LangChain-green)



## 📖 Overview



**StudyAI** is a cutting-edge Generative AI application designed to act as an intelligent study assistant. It leverages **LangChain** and **Groq's LPU** (Language Processing Unit) for ultra-fast inference capabilities. 



Beyond the AI application itself, this project demonstrates a complete **End-to-End LLMOps Pipeline**. It creates a fully automated workflow where code changes are instantly tested, built, containerized, and deployed to a Kubernetes cluster using the **GitOps** methodology.



---



## 🔗 Live Project Links



| Service | Status | Link |

| :--- | :--- | :--- |

| **Jenkins Pipeline** | 🟢 **Active** | [View CI/CD Logs](http://YOUR_JENKINS_URL/job/study%20ai/) |

| **Argo CD Dashboard** | 🟢 **Synced** | [View GitOps Sync](https://YOUR_ARGOCD_URL) |

| **AI Application** | 🟢 **Live** | [Launch StudyAI](http://YOUR_APP_URL) |



---



## 🏗️ Architecture & CI/CD Pipeline



The project follows a modern **GitOps** workflow, ensuring that the "Git Repository" is the single source of truth for the infrastructure.



```mermaid

graph LR

    A[Developer] -- Push Code --> B[GitHub Repo]

    B -- Webhook --> C[Jenkins CI]

    C -- Build Image --> D[Docker Hub]

    C -- Update Manifest [skip ci] --> B

    B -- Sync --> E[Argo CD]

    E -- Deploy --> F[Kubernetes Cluster]

    F -- Expose --> G[User]
