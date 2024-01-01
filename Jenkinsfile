pipeline {
    agent any
    stages {
        stage('Git Master Branch') {
            steps {
                git credentialsId: 'dali05', url: 'https://github.com/dali0505/catprices.git'
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
        stage('Load Docker Image into Kubernetes') {
            steps {
                script {
                    // Charger l'image Docker locale dans le cluster Kubernetes
                    sh 'docker save -o catprices.tar dali05/catprices:latest'

                    // Obtenir le namespace actuel du contexte Kubernetes
                    def currentNamespace = sh(script: 'kubectl config view --minify --output "jsonpath={..namespace}"', returnStdout: true).trim()

                    // Obtenir le nom du premier pod correspondant aux étiquettes spécifiées
                    def podName = sh(script: 'kubectl get pods -n $currentNamespace -l app=catprices-app -o jsonpath="{.items[0].metadata.name}"', returnStdout: true).trim()

                    sh "kubectl cp catprices.tar $podName:/path/to/catprices.tar -n $currentNamespace"
                    sh "kubectl exec -n $currentNamespace $podName -- sh -c 'docker load -i /path/to/catprices.tar'"
                }
            }
        }
        stage('Deploy To Test') {
            steps {
                script {
                    // Appliquer le déploiement Kubernetes en utilisant kubectl
                    sh 'kubectl apply -f deployment.yaml --validate=false'
                }
            }
        }
    }
}
