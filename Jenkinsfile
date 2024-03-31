pipeline {
    agent any

    environment {
        dockerImageName = "shehzadaslamoza/todo-app"
    }

    stages {
        stage('Checkout Source') {
    steps {
        // Checkout the source code from your Git repository
        git branch: 'main', url: 'https://github.com/ShehzadAslamOza/devops-jenkins-todoapp.git'}}

        stage('Build Image') {
            steps {
                script {
                    // Build the Docker image using the Dockerfile in your repository
                    dockerImage = docker.build dockerImageName
                }
            }
        }

        stage('Pushing Image') {
            environment {
                registryCredential = 'dockerhub-credentials'
            }
            steps {
                script {
                    // Push the Docker image to Docker Hub
                    docker.withRegistry('https://registry.hub.docker.com', registryCredential) {
                        dockerImage.push("latest")
                    }
                }
            }
        }

        stage('Deploying todoapp container to Minikube') {
            steps {
                script {
                    // Apply the Kubernetes deployment and service configurations
                    kubernetesDeploy(configs: "k8s/todo-app-deployment.yml", "k8s/secrets.yaml")
                }
            }
        }
    }
}
