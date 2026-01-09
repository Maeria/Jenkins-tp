pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                bat './gradlew clean build'
                bat './gradlew javadoc'
                archiveArtifacts 'build/libs/*.jar'
                archiveArtifacts 'build/docs/javadoc/**'
            }
        }

        stage('Tests Unitaires') {
            steps {
                bat './gradlew test'
                archiveArtifacts 'build/test-results/**/*.xml'
                archiveArtifacts 'build/reports/tests/**/*.html'
            }
        }

        stage('Cucumber Report') {
            steps {
                //bat './gradlew cucumber'
                archiveArtifacts 'reports/example-report.json'
            }
        }

        stage('Analyse du Code') {
            steps {
                withSonarQubeEnv('sonar') {
                    bat './gradlew sonarqube'
                }
            }
        }

        stage('Code Quality') {
            steps {
                script {
                    echo "Vérification Quality Gate"
                    def qg = waitForQualityGate()
                    if (qg.status != 'OK') {
                        error "Quality Gate échoué: ${qg.status}"
                    } else {
                        echo "Quality Gate OK"
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                bat './gradlew publish -PMAVEN_USER=%MAVEN_USER% -PMAVEN_PASSWORD=%MAVEN_PASSWORD%'
            }
        }

        stage('Notification') {
            steps {
                slackSend(
                    channel: '#general',
                    color: 'good',
                    message: "Le déploiement a réussi !",
                    tokenCredentialId: 'slack-bot-token'
                )
            }
        }
    }

    post {
        always {
            echo "Phase Test terminée"
        }
        success {
            echo "Tous les tests ont réussi"
        }
        failure {
            echo "Échec dans une ou plusieurs phases"
            slackSend(
                channel: '#general',
                color: 'danger',
                message: "Pipeline échoué !",
                tokenCredentialId: 'slack-bot-token'
            )
        }
    }
}
