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
