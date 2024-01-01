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
              withCredentials([string(credentialsId: 'Docker', variable: 'DOCKER_CREDENTIALS')]) {
              sh "echo $DOCKER_CREDENTIALS | docker login -u mon_nom_utilisateur --password-stdin"
              sh "docker push dali/catprices:latest"
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
