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
                withSonarQubeEnv('sonar') {
                    bat './gradlew sonarqube'
                }
            }
        }
        stage('Code Quality') {
         steps {

                script {
                 echo " Vérification Quality Gate"
                                def qg = waitForQualityGate()
                                if (qg.status != 'OK') {
                                    error " Quality Gate échoué: ${qg.status}"
                                } else {
                                    echo " Quality Gate OK"
                                }

                }
            }
        }
    stage('Deploy') {
        steps {
            bat './gradlew publish -PMAVEN_USER=%MAVEN_USER% -PMAVEN_PASSWORD=%MAVEN_PASSWORD%'
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
      echo "Échec  "
         }
      }

}
