pipeline {
    agent {
        docker { 
            image 'python:3.9-slim' 
            args '-u root' 
        }
    }
    stages {
        stage('Setup') {
            steps {
                sh 'apt-get update && apt-get install -y wget curl git'
            }
        }
        stage('SAST - Semgrep') {
            steps {
                sh 'pip install semgrep'
                // On scanne et on bloque si erreur
                sh 'semgrep scan --config=auto --error' 
            }
        }
        stage('SCA - Trivy') {
            steps {
                // Installation de Trivy
                sh 'curl -sfL https://raw.githubusercontent.com/aquasecurity/trivy/main/contrib/install.sh | sh -s -- -b /usr/local/bin'
                // Scan des dépendances (bloquant sur HIGH/CRITICAL)
                sh 'trivy fs . --exit-code 1 --severity HIGH,CRITICAL'
            }
        }
    }
}
