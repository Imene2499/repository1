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

        stage('Code Analysis') {
                    steps {
                        echo 'Analyzing code quality with SonarQube..'
                        withSonarQubeEnv('sonar') {
                           bat './gradlew sonarqube'  // Execute SonarQube analysis
                        }
                    }
        }

        stage('Quality Gate') {
                steps {
                    echo 'Waiting for SonarQube quality gate...'
                    // Wait for the SonarQube analysis to complete and check the quality gate status
                    waitForQualityGate abortPipeline: true  // If the gate fails, the pipeline will be aborted
                    }
        }
    }
}
