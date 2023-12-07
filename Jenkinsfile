pipeline {
    agent any
    stages {

        stage('Init') {
            steps {
                sh '''
                docker network create jenk-network || echo "Network Already Exists"
                docker stop flask-app || echo "flask-app Not Running"
                docker stop nginx || echo "nginx Not Running"
                docker rm flask-app || echo "flask-app Not Running"
                docker rm nginx || echo "nginx Not Running"
                '''
            }
        }

        stage('Build') {
            steps {
                sh '''
                docker build -t lavyyndocker/flask-jenk .
                docker build -t lavyyndocker/nginx-jenk ./nginx
                '''
            }
        }
        stage('Push') {
            steps {
                sh '''
                docker push lavyyndocker/flask-jenk .
                docker push lavyyndocker/nginx-jenk
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                docker run -d --name flask-app --network jenk-network lavyyndocker/flask-jenk
                docker run -d -p 80:80 --name nginx --network jenk-network lavyyndocker/nginx-jenk
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