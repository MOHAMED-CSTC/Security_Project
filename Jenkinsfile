pipeline {
    agent any

    stages {
        stage('SCM') {
            steps {
                checkout scm
            }
        }
        stage('Composer Audit') {
            steps {
                // Exécuter Composer Audit pour vérifier les vulnérabilités sans installer les dépendances
                sh 'composer audit'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    def scannerHome = tool 'SonarScanner'
                    withSonarQubeEnv() {
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
                }
            }
        }

        stage('OWASP Dependency-Check Analysis') {
            steps {
                // Exécute l'analyse OWASP Dependency-Check
                dependencyCheck additionalArguments: '--scan ./ --out ./dependency-check-report', odcInstallation: 'dependency-check'

                // Publie les résultats de l'analyse sous forme de rapport XML
                dependencyCheckPublisher pattern: 'dependency-check-report/dependency-check-report.xml'
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed'
        }
        failure {
            echo 'Pipeline failed. Check logs for details.'
        }
        success {
            echo 'Pipeline succeeded.'
        }
    }
}