pipeline {
    agent any
    stages {

        stage('Build') {
            steps {
                sh '''
                docker build -t lavyyndocker/task1-kube:latest -t lavyyndocker/task1-kube:v${BUILD_NUMBER} .
                '''
            }
        }

        stage('Push') {
            steps {
                sh '''
                docker push lavyyndocker/task1-kube:latest
                docker push lavyyndocker/task1-kube:v${BUILD_NUMBER}
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                kubectl apply -f ./kubernetes
                kubectl set image deployment/flask-deployment task1=lavyyndocker/task1-kube:v${BUILD_NUMBER}
                '''
            }
        }

        stage('CleanUp') {
            steps {
                sh '''
                docker system prune -f 
                '''
            }
        }
    }

}