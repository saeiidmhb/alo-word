pipeline {
    agent { label 'all' }

    stages {
        stage('Clone WordPress') {
            steps {
                git branch: "main", url: 'https://github.com/saeiidmhb/wordpress.git'
            }
        }

        stage('Build and Push Docker Image') {
            steps {
                script {
                     sh 'docker login -u saeiidmhb -pddk2qntr'
                    dockerImage = docker.build("saeiidmhb/wordpress:latest")
                    withDockerRegistry(credentialsId: "dockerhub-credentials", url: "") {
                        dockerImage.push()
                    }
                }
            }
        }

        stage('Deploy to Server') {
            steps {
                sshagent(credentials: ['ssh-credential']) {
                    sh 'sshpass -p "qazwsx" ssh -o StrictHostKeyChecking=no alo@192.168.39.128 "docker compose -f /opt/wordpress/docker-compose.yml 
pull"'
                    sh 'sshpass -p "qazwsx" ssh -o StrictHostKeyChecking=no alo@192.168.39.128 "docker compose -f /opt/wordpress/docker-compose.yml 
down"'
                    sh 'sshpass -p "qazwsx" ssh -o StrictHostKeyChecking=no alo@192.168.39.128 "docker compose -f /opt/wordpress/docker-compose.yml up 
-d"'
                }
            }
        }
    }
}
