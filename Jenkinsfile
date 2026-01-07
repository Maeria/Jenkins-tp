pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                bat './gradlew clean build'
            }
        }
        stage('Test') {
            steps {
                bat './gradlew test'
            }
        }
        stage('Jacoco Report') {
            steps {
                bat './gradlew jacocoTestReport'
            }
        }
         stage('Cucumber Report') {
                    steps {
                        bat './gradlew generateCucumberReports'
                    }
                }
          stage('SonarQube Analysis') {
                     steps {
                         withSonarQubeEnv('SonarQube11') { // nom de ton Sonar installé dans Jenkins
                             bat './gradlew sonarqube'
                         }
                     }
                 }
                 stage('Quality Gate') {
                     steps {
                         timeout(time: 1, unit: 'MINUTES') {
                             waitForQualityGate abortPipeline: true
                         }
                     }
                 }
    }
}
