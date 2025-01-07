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
            post {
                failure {
                    echo 'Test failed. Sending notification...'
                    bat './gradlew sendFailureNotification'  // Remplacez par une tâche pour envoyer une notification en cas d'échec
                }
            }
        }

        stage('Code Analysis') {
            steps {
                echo 'Analyzing code quality with SonarQube..'
                withSonarQubeEnv('sonar') {
                    bat './gradlew sonarqube'  // Execute SonarQube analysis
                }
            }
            post {
                failure {
                    echo 'Code analysis failed. Sending notification...'
                    bat './gradlew sendFailureNotification'  // Notification en cas d'échec
                }
            }
        }

        stage('Quality Gate') {
            steps {
                echo 'Waiting for SonarQube quality gate...'
                waitForQualityGate abortPipeline: true  // If the gate fails, the pipeline will be aborted
            }
            post {
                failure {
                    echo 'Quality Gate failed. Sending notification...'
                    bat './gradlew sendFailureNotification'  // Notification en cas d'échec
                }
            }
        }

        stage('Build') {
            steps {
                echo 'Building Project..'
                bat './gradlew javadoc'
                echo 'Archiving Artifacts...'
                archiveArtifacts artifacts: '**/build/libs/TP5-1.0-SNAPSHOT.jar, **/build/tmp/javadoc/**/*', fingerprint: true
            }
            post {
                failure {
                    echo 'Build failed. Sending notification...'
                    bat './gradlew sendFailureNotification'  // Notification en cas d'échec
                }
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying Project..'
                bat './gradlew publish'
            }
            post {
                failure {
                    echo 'Deployment failed. Sending notification...'
                    bat './gradlew sendFailureNotification'  // Notification en cas d'échec
                }
            }
        }

        stage('Notification') {
            steps {
                echo 'Sending Notification via slack..'
                bat './gradlew postBuiltSucceedToSlack'

                 echo 'Sending Notification via email..'
                    mail to: 'imenelouni2004@gmail.com',
                         from: 'li_louni@esi.dz',
                         subject: 'Project deployement Completed Successfully',
                         body: 'The Project deployement has completed successfully.'
            }
        }
    }
}
