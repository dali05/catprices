pipeline {
    agent any
    stages {
        stage('Git Master Branch') {
            steps {
                git credentialsId: 'dali05', url: 'https://github.com/dali05/catprices.git'
            }
        }
        stage('Build') {
            steps {
                withMaven(maven: 'apache-maven-3.8.4') {
                    bat 'mvn clean install'
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                bat 'docker build -t dali05/catprices:latest .'
            }
        }
        stage('Push Docker Image') {
            steps {
             withCredentials([usernamePassword(credentialsId: 'Docker', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
             sh "docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD"
             bat 'docker push dali05/catprices:latest'
             }
            }
        }
        stage('Deploy To Test') {
            steps {
                bat 'kubectl apply -f deployment.yaml'
            }
        }
    }
}
