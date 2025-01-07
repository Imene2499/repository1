pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
                bat './gradlew build'
            }
        }
        stage('Unit Tests') {
            steps {
                echo 'Running unit tests..'
                bat './gradlew test'
            }
        }
        stage('Archive Test Results') {
            steps {
                echo 'Archiving test results..'
                archiveArtifacts artifacts: 'build/test-results/**/*.xml', allowEmptyArchive: true
            }
        }
        stage('Generate Cucumber Reports') {
            steps {
                echo 'Generating Cucumber reports..'
                bat './gradlew generateCucumberReports' // Replace this with your actual Gradle task for Cucumber reports
            }
        }

        stage('Generate Cucumber Reports') {
                    steps {
                        echo 'Generating Cucumber reports..'
                        bat './gradlew generateCucumberReports' // Replace this with your actual Gradle task for Cucumber reports
                    }
        }
//         stage('Code Analysis') {
//                     steps {
//                         echo 'Analyzing code quality with SonarQube..'
//                         bat './gradlew sonarqube' // Runs the SonarQube analysis task
//                     }
//         }
    }
}
