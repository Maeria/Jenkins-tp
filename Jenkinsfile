pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                bat './gradlew clean build'
            }
        }
        stage('tests unitaires') {
            steps {
                bat './gradlew test'
                //archiver
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
        stage('Analyse du code') {
            steps {
                withSonarQubeEnv('sonar') {  // <-- Attention ici
                    bat './gradlew sonarqube'
                }
            }
        }
        stage('Code Quality') {
         steps {
                // Toutes les instructions Groovy doivent être dans "script {}"
                script {
                 echo "⏳ Vérification Quality Gate"
                                def qg = waitForQualityGate()
                                if (qg.status != 'OK') {
                                    error " Quality Gate échoué: ${qg.status}"
                                } else {
                                    echo " Quality Gate OK"
                                }

                }
            }
        }



    }
    post {
      always {
     echo "Phase Test terminée"
       }
        success {
         echo "Tous les tests unitaires et Cucumber ont réussi"
       }
      failure {
      echo "Échec dans les tests unitaires ou Cucumber "
         }
      }

}
