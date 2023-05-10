pipeline {
    agent any

    stages {
        stage('Clone WordPress') {
            steps {
                git 'https://github.com/saeiidmhb/wordpress.git'
            }
        }

        stage('Build and Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', passwordVariable: 'DOCKERHUB_PASSWORD', usernameVariable: 
'DOCKERHUB_USERNAME')]) {
                    sh 'docker login -u $DOCKERHUB_USERNAME -p $DOCKERHUB_PASSWORD'
                    sh 'docker build -f ./wordpress/Dockerfile . -t saeiidmhb/wordpress:latest'
                    sh 'docker push saeiidmhb/wordpress:latest'
                }
            }
        }

        stage('Deploy to Server') {
            steps {
                sshagent(credentials: ['ssh-credentials-id']) {
                    sh 'ssh root@192.168.51.5 "docker-compose -f /opt/wordpress/docker-compose.yml pull"'
                    sh 'ssh root@192.168.51.5 "docker-compose -f /opt/wordpress/docker-compose.yml down"'
                    sh 'ssh root@192.168.51.5 "docker-compose -f /opt/wordpress/docker-compose.yml up -d"'
                }
            }
        }
    }
}

