pipeline {
    agent any

    stages {
        stage('SCM') {
            steps {
                checkout scm
            }
        }

        stage('OWASP Dependency-Check Analysis') {
            steps {
                // Exécute l'analyse OWASP Dependency-Check avec génération de rapports XML et HTML
                dependencyCheck additionalArguments: '--scan ./ --format "ALL" --out ./dependency-check-report', odcInstallation: 'dependency-check'

                // Publie les résultats de l'analyse sous forme de rapport XML
                dependencyCheckPublisher pattern: 'dependency-check-report/dependency-check-report.xml'
            }
        }

        stage('Publish HTML Report') {
            steps {
                // Publie le rapport HTML généré
                publishHTML(target: [
                    allowMissing: false,
                    alwaysLinkToLastBuild: true,
                    keepAll: true,
                    reportDir: 'dependency-check-report',
                    reportFiles: 'dependency-check-report.html',
                    reportName: 'OWASP Dependency-Check HTML Report'
                ])
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
