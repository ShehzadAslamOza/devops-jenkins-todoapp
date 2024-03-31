pipeline {
    agent any

    environment {
        dockerImageName = "shehzadaslamoza/todo-app"
        kubeconfigCredentialId = 'kubeconfig-file'
    }

    stages {
        stage('Checkout Source') {
            steps {
                git branch: 'main', url: 'https://github.com/ShehzadAslamOza/devops-jenkins-todoapp.git'
            }
        }

        stage('Build Image') {
            steps {
                script {
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
                    docker.withRegistry('https://registry.hub.docker.com', registryCredential) {
                        dockerImage.push("latest")
                    }
                }
            }
        }

        stage('Deploying todoapp container to Minikube') {
            steps {
                script {
                    // Set KUBECONFIG environment variable to use kubeconfig credential
                    withCredentials([file(credentialsId: kubeconfigCredentialId, variable: 'KUBECONFIG')]) {
                        // Apply the Kubernetes deployment and service configurations
                        bat 'kubectl apply -f ./k8s/secrets.yml'
                        bat 'kubectl apply -f ./k8s/todo-app-deployment.yml'
                    }
                }
            }
        }
    }
}
