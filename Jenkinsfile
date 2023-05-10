pipeline {
    agent any

    stages {
        stage('Clone WordPress Theme') {
            steps {
                git 'https://github.com/saeiidmhb/wordpress.git'
            }
        }

        stage('Build and Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', passwordVariable: 'DOCKERHUB_PASSWORD', usernameVariable: 
'DOCKERHUB_USERNAME')]) {
                    sh 'docker login -u $DOCKERHUB_USERNAME -p $DOCKERHUB_PASSWORD'
                    sh 'docker build -t saeiidmhb/wordpress:latest ./wordpress/Dockerfile'
                    sh 'docker push saeiidmhb/wordpress:latest'
                }
            }
        }

        stage('Deploy to Server') {
            steps {
                sshagent(['ssh-credentials']) {
                    sh 'ssh root@192.168.51.5 "docker-compose pull"'
                    sh 'ssh root@192.168.51.5 "docker-compose down"'
                    sh 'ssh root@192.168.51.5 "docker-compose up -d"'
                }
            }
        }
    }
}

