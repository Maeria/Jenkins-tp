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
    }
}
