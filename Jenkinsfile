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
                bat 'java -cp "path\\to\\cucumber-jars;build\\classes" io.cucumber.core.cli.Main --plugin json:reports/cucumber.json --glue com.belhadjmaria.stepdefs src/test/resources/features'
                bat 'java -jar path\\to\\cucumber-reporting.jar --input reports/cucumber.json --output reports/cucumber-html'
            }
        }

          stage('SonarQube Analysis') {
                     steps {
                             bat './gradlew sonarqube'

                     }
                 }

    }
}
