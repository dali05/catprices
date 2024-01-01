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
}
