pipeline {
    agent any
    tools {
        nodejs 'nodejs'
    }
    environment {
        DOCKER = credentials('d8fb1bc4-19a1-4807-89f4-399778d94b95')
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main'
                url: 'https://github.com/20127050/CI-CD-Online-Academy.git'
            }
        }
        stage('Build project') {
            steps {
                sh 'npm install'
            }
        }
        stage('Docker build image') {
            steps {
                sh 'docker build -t online_academy:latest .'
            }
        }
        stage('Docker push image') {
            steps {
                sh 'docker tag online_academy:latest $DOCKER_USR/online_academy:latest'

                sh 'echo $DOCKER_PSW | docker login -u $DOCKER_USR --password-stdin'
                sh 'docker push $DOCKER_USR/online_academy:latest'
            }
        }
        stage('Run remote docker container') {
            steps {
                sh 'ssh -i /home/keys/192.168.1.10/id_rsa -o StrictHostKeyChecking=no user-20127540@192.168.1.10 docker run -d -p 5000:8080 --name online_academy $DOCKER_USR/online_academy:latest'
            }
        }
    }
}