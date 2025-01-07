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
                        bat './gradlew sonarqube' // Runs the SonarQube analysis task
                    }
        }

        stage('Quality Gate') {
                    steps {
                        script {
                            // Wait for SonarQube Quality Gate result
                            def qg = waitForQualityGate()  // This method waits for the SonarQube quality gate result
                            if (qg.status != 'OK') {
                                // If the Quality Gate is not OK, we stop the pipeline with a failure message
                                error "Quality Gate failed! Exiting the pipeline."
                            } else {
                                echo "Quality Gate passed!"
                            }
                        }
                    }
        }
    }
}
