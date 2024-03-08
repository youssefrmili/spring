pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/youssefrmili/spring.git'
            }
        }

        stage('SAST') {
            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'd54a9dca-6055-46c1-b88f-f8d75781979d') {
                        sh 'docker start sonarqube'
                        sh 'mvn sonar:sonar'
                        sh 'cat target/sonar/report.txt'
                    }
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
