pipeline {
    agent any

    environment {
        // 1. match your DockerHub username exactly
        DOCKER_HUB_USER = 'adityapareek01' 
        // 2. This name must match what is in deploy.yml later
        DOCKER_IMAGE_NAME = 'calculator-app' 
        GITHUB_REPO_URL = 'https://github.com/Aditya01237/mini-project-calculator.git'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: "${GITHUB_REPO_URL}"
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${DOCKER_HUB_USER}/${DOCKER_IMAGE_NAME}:latest ."
            }
        }

        stage('Push Docker Images') {
            steps {
                // FIXED: Using withCredentials block for safety
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                    sh "docker push ${DOCKER_HUB_USER}/${DOCKER_IMAGE_NAME}:latest"
                }
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                // Ensure we use the inventory file we created
                sh "ansible-playbook -i inventory deploy.yml"
            }
        }
    }
}
