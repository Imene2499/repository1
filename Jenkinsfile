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
                cucumber '**/reports/*.json'
            }
            post {
                failure {
                mail to: 'imenelouni2004@gmail.com',
                     from: 'li_louni@esi.dz',
                     subject: 'Test failed',
                     body: 'Test failed.'

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
                    mail to: 'imenelouni2004@gmail.com',
                         from: 'li_louni@esi.dz',
                         subject: 'Code Analysis failed',
                         body: 'Code Analysis failed.'
                }
            }
        }

        stage('Quality Gate') {
            steps {
                echo 'Waiting for SonarQube quality gate...'
                waitForQualityGate abortPipeline: true
            }
            post {
                failure {
                    mail to: 'imenelouni2004@gmail.com',
                         from: 'li_louni@esi.dz',
                         subject: 'Quality Gate failed',
                         body: 'Quality Gate failed.'
                }
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
            post {
                failure {
                   mail to: 'imenelouni2004@gmail.com',
                        from: 'li_louni@esi.dz',
                        subject: 'Build failed ',
                        body: 'Build failed.'
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
                   mail to: 'imenelouni2004@gmail.com',
                        from: 'li_louni@esi.dz',
                        subject: 'Deployement failed',
                        body: 'Deployement failed.'
                }
            }
        }

        stage('Notification') {
            steps {
                echo 'Sending Notification via Slack..'
                slackSend channel: '#all-esi', message: 'deploy completed'

                 echo 'Sending Notification via email..'
                    mail to: 'imenelouni2004@gmail.com',
                         from: 'li_louni@esi.dz',
                         subject: 'Project deployement Completed Successfully',
                         body: 'The Project deployement has completed successfully.'
            }
        }
    }
}
