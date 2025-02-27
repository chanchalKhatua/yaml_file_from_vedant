pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = "us-east-1"
    }

    stages {
        stage('Checkout Source') {
            steps {
                git branch: 'e2e_project', url: 'https://github.com/vedantsharmascaler/testing_repo.git'
            }
        }

        stage('Build Image') {
            steps {
                script {
                    sh 'docker build -t vedant120/react-app:latest .'
                }
            }
        }
        stage('Login') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', passwordVariable: 'DOCKERHUB_PSW', usernameVariable: 'DOCKERHUB_USR')]) {
                        sh 'echo $DOCKERHUB_PSW | docker login -u $DOCKERHUB_USR --password-stdin'
                    }
                }
            }
        }

        stage('Push') {
            steps {
                sh 'docker push vedant120/react-app:latest'
            }
        }

        stage('Deploy to EKS') {
            steps {
                sh 'kubectl config use-context my-eks-cluster'
                sh 'kubectl apply -f deployment.yaml'
                sh 'kubectl apply -f service.yaml'
            }
        }
    }
}
