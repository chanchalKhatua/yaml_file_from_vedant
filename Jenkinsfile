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
                    sh 'sudo docker build -t vedant120/react-app:v2 .'
                }
            }
        }
        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh 'echo "$PASS" | sudo docker login -u "$USER" --password-stdin'
                }
            }
        }

        stage('Push') {
            steps {
                sh 'sudo docker push vedant120/react-app:v2'
            }
        }

        stage('Deploy to EKS') {
            steps {
                sh 'kubectl config use-context arn:aws:eks:us-east-1:266735832911:cluster/jenkinsProject'
                sh 'kubectl apply -f deployment.yaml'
                sh 'kubectl apply -f service.yaml'
            }
        }
    }
}
