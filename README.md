# CI-CD-Pipeline-using-Jenkins-SonarQube-Docker-and-Argo-CD




🧩 Project Overview

This project demonstrates a complete CI/CD pipeline for automating the process of building, testing, analyzing, containerizing, and deploying a Spring Boot application using modern DevOps tools.
Whenever new code is pushed to the repository, Jenkins automatically runs the pipeline — ensuring quality checks, Docker image updates, and seamless deployment to Kubernetes through Argo CD.


<img width="1139" height="436" alt="Screenshot 2025-08-19 143347" src="https://github.com/user-attachments/assets/3c7ff652-6861-4f66-abd8-70dc89bfcf5b" />
<img width="1877" height="833" alt="Screenshot 2025-08-19 143430" src="https://github.com/user-attachments/assets/6b690b2b-0e3e-4111-a59b-0669849ee337" />
<img width="1139" height="436" alt="Screenshot 2025-08-19 143347" src="https://github.com/user-attachments/assets/3bbbffdc-d0ed-4142-90c5-813dabe9e670" />


🛠️ Tech Stack & Tools Used
Tool	Purpose
Jenkins	CI/CD automation
Maven	Build management for the Java project
SonarQube	Static code analysis for quality and vulnerabilities
Docker	Containerization of the Spring Boot app
DockerHub	Registry for storing Docker images
Argo CD	Continuous Deployment to Kubernetes
Kubernetes (K8s)	Application orchestration and deployment
GitHub	Source code & manifests repository
Slack / Email (optional)	Notification system
⚙️ Project Flow

Developer Pushes Code

The code is pushed to the GitHub repository.

A webhook triggers Jenkins automatically.

Jenkins Pipeline Triggered

Jenkins fetches the latest source code.

Build and Test (Maven)

Maven compiles the Java Spring Boot project and runs tests.

If successful, the build moves to the next stage.

Static Code Analysis (SonarQube)

SonarQube checks for code quality, security issues, and code smells.

If it passes the quality gate, the process continues.

Docker Image Creation and Push

A new Docker image of the application is built.

The image is tagged with the Jenkins build number and pushed to DockerHub.

Update Deployment Manifest

Jenkins updates the Kubernetes deployment YAML file with the new Docker image tag.

The updated manifest is committed and pushed back to GitHub.

Argo CD Deployment

Argo CD monitors the manifests repository.

It detects the updated manifest and automatically deploys the new image to the Kubernetes cluster.

Reporting & Notification (Optional)

Jenkins sends build and test reports through Slack or Email.

🔁 End-to-End Flow Diagram
Source Code → Jenkins → Maven Build → SonarQube → Test → 
Docker Build & Push → Update Manifest Repo → Argo CD → Kubernetes Deployment

🧠 Pipeline Stages Explained (From Jenkinsfile)
1️⃣ Checkout

Pulls the source code from the main branch of your GitHub repository.

git branch: 'main', url: 'https://github.com/iam-veeramalla/Jenkins-Zero-To-Hero.git'

2️⃣ Build and Test

Uses Maven to clean, compile, and package the Spring Boot project.

cd java-maven-sonar-argocd-helm-k8s/spring-boot-app
mvn clean package

3️⃣ Static Code Analysis

Performs code quality checks using SonarQube.

mvn org.sonarsource.scanner.maven:sonar-maven-plugin:4.0.0.4121:sonar \
 -Dsonar.token=$SONAR_AUTH_TOKEN \
 -Dsonar.host.url=$SONAR_URL

4️⃣ Build and Push Docker Image

Builds a Docker image and pushes it to DockerHub with the build number as a tag.

dockerImage = docker.build("abhishekf5/spring-boot-app:${BUILD_NUMBER}")
docker.withRegistry('', 'dockerhub') {
    dockerImage.push()
}

5️⃣ Update Deployment File

Updates the Kubernetes deployment YAML file with the new image version and pushes it back to GitHub.

sed -i "s/replaceImageTag/${BUILD_NUMBER}/g" java-maven-sonar-argocd-helm-k8s/spring-boot-app-manifests/deployment.yml
git add java-maven-sonar-argocd-helm-k8s/spring-boot-app-manifests/deployment.yml
git commit -m "Update deployment image to version ${BUILD_NUMBER}"
git push origin main

🧾 Jenkinsfile Summary

The pipeline runs inside a Docker agent (maven-abhishek-docker-agent:v2).

Automatically performs build, test, SonarQube scan, image push, and manifest update.

Uses environment variables, credentials, and GitHub tokens securely.

Integrates with Argo CD for continuous deployment to Kubernetes.

🧭 Final Outcome

✅ Fully automated CI/CD pipeline
✅ Continuous integration using Jenkins, Maven & SonarQube
✅ Continuous delivery using DockerHub, GitHub & Argo CD
✅ Deployed and managed on Kubernetes
