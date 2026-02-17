pipeline {
    agent any

    environment {
        // Your Docker Hub details
        DOCKER_HUB_USER = 'pareekaditya01'
        DOCKER_IMAGE_NAME = 'calculator-app'
        // Your GitHub URL
        GITHUB_REPO_URL = 'https://github.com/Aditya01237/mini-project-calculator.git'
    }

    stages {
        stage('Checkout') {
            steps {
                // Pulls code from your repository
                git branch: 'master', url: "${GITHUB_REPO_URL}"
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Builds the image using the Docker Pipeline plugin syntax
                    // Matches Screenshot 10.40.54
                    docker.build("${DOCKER_HUB_USER}/${DOCKER_IMAGE_NAME}:latest", '.')
                }
            }
        }

        stage('Push Docker Images') {
            steps {
                script {
                    // Logs in using the 'DockerHubCred' ID we created earlier
                    // Matches Screenshot 10.41.09
                    docker.withRegistry('', 'DockerHubCred') {
                        sh "docker push ${DOCKER_HUB_USER}/${DOCKER_IMAGE_NAME}:latest"
                    }
                }
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                // Runs the playbook using the Ansible plugin
                // Matches Screenshot 10.41.17
                ansiblePlaybook installation: 'ansible', playbook: 'deploy.yml', inventory: 'inventory'
            }
        }
    }
}
