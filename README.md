🔄 Project Overview
You’ll build a CI/CD pipeline for the simple Node.js web app using:

Git for version control.
Jenkins for CI/CD automation.
Docker for containerization.
Shell scripts for deployment and automation.
Nginx or Apache as a web server (optional).


📋 Key DevOps Concepts Covered
Version control with Git.
Continuous Integration (CI) with Jenkins.
Continuous Deployment (CD) using Docker and shell scripts.
Automation with Bash scripting.
Containerization with Docker.
Monitoring basics with Jenkins plugins.


📂 Project Structure


simple-node-app/
├── app.js
├── package.json
├── package-lock.json
├── Dockerfile
├── deploy.sh
├── .gitignore
├── Jenkinsfile
└── README.md


📌 Step 1: Create a Git Repository

Initialize Git and create a new repository on GitHub.
Push your code to GitHub:

git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/your-username/simple-node-app.git
git push -u origin main


📌 Step 2: Create a Dockerfile

dockerfile

# Dockerfile
FROM node:14

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

EXPOSE 5000

CMD ["npm", "start"]


📌 Step 3: Create a deploy.sh Script for Deployment

This script will:

Stop any existing Docker containers for this app.
Remove the old Docker image.
Build a new Docker image.
Run a new container.
deploy.sh


#!/bin/bash

APP_NAME="simple-node-app"
PORT=5000

echo "🚀 Stopping existing Docker containers..."
docker stop $APP_NAME || true

echo "🗑 Removing existing Docker containers..."
docker rm $APP_NAME || true

echo "🗑 Removing old Docker images..."
docker rmi $APP_NAME:latest || true

echo "🐳 Building a new Docker image..."
docker build -t $APP_NAME:latest .

echo "🚀 Running a new Docker container..."
docker run -d -p $PORT:5000 --name $APP_NAME $APP_NAME:latest

echo "✅ Deployment completed! Visit http://localhost:$PORT"
Make it executable:


chmod +x deploy.sh


📌 Step 4: Create a Jenkinsfile for CI/CD Pipeline

This file defines the pipeline steps:

Clone the repository.
Build the Docker image.
Run tests.
Deploy using the deploy.sh script.
Jenkinsfile


pipeline {
    agent any
    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/your-username/simple-node-app.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t simple-node-app:latest .'
                }
            }
        }
        stage('Run Tests') {
            steps {
                sh 'echo "Running tests..."'
                sh 'npm test || echo "No tests defined"'
            }
        }
        stage('Deploy') {
            steps {
                sh './deploy.sh'
            }
        }
    }
}


📌 Step 5: Configure Jenkins for CI/CD

Install Jenkins and plugins:
Docker Pipeline.
Git.
Create a new pipeline job in Jenkins:
In Pipeline script from SCM, use your Git repository URL.
Run the build to test the pipeline.


📌 Step 6: Test the CI/CD Pipeline

Push code to GitHub.
Jenkins should automatically:
Clone the code.
Build a Docker image.
Run tests.
Deploy the app.
Check the app at: http://localhost:5000.


📌 Step 7: Monitor and Logs

Use Jenkins plugins for:
Build logs.
Docker container logs.


🎯 Key Learnings from This Project
CI/CD: Automated builds and deployments.
Containerization: Docker skills.
Shell scripting: Automating tasks.
Jenkins: Pipeline management.
