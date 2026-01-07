pipeline {
    agent any
    stages {
        stage('Build') {
            steps {

                bat 'gradlew build'

            }
        }
        stage('Jacoco Coverage') {
            steps {
                bat 'gradlew jacocoTestReport'
            }
            post {
                always {
                    jacoco(execPattern: '**/build/jacoco/*.exec', classPattern: '**/classes/java/main', sourcePattern: '**/src/main/java', exclusionPattern: '')
                }
            }
        }
        stage('Cucumber'){
        steps{
        }
        }

    }
}
