pipeline {
    agent any

    environment {
        DOCKER_HUB_USER = 'adityapareek01'
        DOCKER_IMAGE_NAME = 'calculator-app'
        GITHUB_REPO_URL = 'https://github.com/Aditya01237/mini-project-calculator.git'
        
        // This automatically pulls the username/password for the ID 'DockerHubCred'
        // and stores them in DOCKER_CREDS_USR and DOCKER_CREDS_PSW
        DOCKER_CREDS = credentials('DockerHubCred')
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: "${GITHUB_REPO_URL}"
            }
        }

        stage('Build Docker Image') {
            steps {
                // "sh" runs a standard shell command
                sh "docker build -t ${DOCKER_HUB_USER}/${DOCKER_IMAGE_NAME}:latest ."
            }
        }

        stage('Push Docker Images') {
            steps {
                // Standard docker login using the secure credentials
                sh 'echo $DOCKER_CREDS_PSW | docker login -u $DOCKER_CREDS_USR --password-stdin'
                sh "docker push ${DOCKER_HUB_USER}/${DOCKER_IMAGE_NAME}:latest"
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                // Run ansible directly from shell to avoid needing the Ansible plugin
                sh "ansible-playbook -i inventory deploy.yml"
            }
        }
    }
}
