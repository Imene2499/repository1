pipeline {
    agent any

    stages {

        stage('Test') {
            steps {
                echo 'Testing..'
                bat './gradlew test'
                echo 'Archiving test results..'
                archiveArtifacts artifacts: 'build/test-results/**/*.xml', allowEmptyArchive: true
                echo 'Generating reports..'
                bat './gradlew generateCucumberReports'
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

        stage('Build') {
                     steps {
                     echo 'Building Project..'
                     bat './gradlew jar'
                     bat './gradlew javadoc'
                     echo 'Archiving Artifacts...'
                     archiveArtifacts artifacts: '**/build/libs/TP5-1.0-SNAPSHOT.jar, **/build/tmp/javadoc/**/*', fingerprint: true
                    }
        }

        stage('Deploy') {
              steps {
                  echo 'Deploying Project..'
                  bat './gradlew publish'
              }
        }
    }
}
