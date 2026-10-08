pipeline {
    agent any
    stages {
        stage('Checkout SCM') {
            steps { checkout scm }
        }
        stage('Check Kubernetes') {
            steps {
                bat 'kubectl version --client'
                bat 'kubectl get nodes'
            }
        }
        stage('Build Docker Image') {
            steps {
                bat 'minikube docker-env --shell cmd > env.bat'
                bat 'call env.bat && docker build -t library-app:1.0 .'
            }
        }
        stage('Deploy to Kubernetes') {
            steps { bat 'kubectl apply -f k8s.yaml' }
        }
        stage('Verify Deployment') {
            steps {
                bat 'kubectl rollout status deployment/library-app'
                bat 'kubectl get pods'
                bat 'kubectl get svc library-service'
            }
        }
    }
    post {
        success { echo 'Library application deployed successfully.' }
        failure { echo 'Deployment failed. Check the console output.' }
    }
}
