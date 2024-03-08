pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/youssefrmili/spring.git'
            }
        }
     stage('SAST') {
            environment {
        SONAR_TOKEN = credentials('6cd23c11-5de1-4a21-8598-d7b608fdc177')
    }
            steps {
                script {
                    sh 'mvn sonar:sonar'
                }
            }
        }
        
        stage('Build and Test') {
            steps {
                script {
                    sh 'mvn clean package'
                }
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t youssefrm/springim:latest .'
                }
            }
        }

        stage('Push Docker Image to Docker Hub') {
            environment {
                DOCKER_CREDENTIALS = credentials('c6ea3088-f7ad-4b7a-a9f6-3614f2de2f25')
            }
            steps {
                sh 'docker login -u $DOCKER_CREDENTIALS_USR -p $DOCKER_CREDENTIALS_PSW'
                sh 'docker push youssefrm/springim:latest'
            }
        }
    }
}
