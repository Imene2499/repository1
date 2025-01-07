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
                    echo 'Test failed. Sending notification via email...'
                    script {
                        sendMail (
                            subject: "Pipeline Failed: ${env.JOB_NAME}",
                            body: """
                                The Jenkins pipeline failed in the stage: Test.

                                Project: ${project.name}
                                Build URL: ${env.BUILD_URL}
                                Please check the logs for more details.
                            """,
                            to: "li_louni@esi.dz",  // Email to send notification
                            from: "li_louni@esi.dz"  // Sender email
                        )
                    }
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
                    echo 'Code analysis failed. Sending notification via email...'
                    script {
                        sendMail (
                            subject: "Pipeline Failed: ${env.JOB_NAME}",
                            body: """
                                The Jenkins pipeline failed in the stage: Code Analysis.

                                Project: ${project.name}
                                Build URL: ${env.BUILD_URL}
                                Please check the logs for more details.
                            """,
                            to: "li_louni@esi.dz",  // Email to send notification
                            from: "li_louni@esi.dz"  // Sender email
                        )
                    }
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
                    echo 'Quality Gate failed. Sending notification via email...'
                    script {
                        sendMail (
                            subject: "Pipeline Failed: ${env.JOB_NAME}",
                            body: """
                                The Jenkins pipeline failed in the stage: Quality Gate.

                                Project: ${project.name}
                                Build URL: ${env.BUILD_URL}
                                Please check the logs for more details.
                            """,
                            to: "li_louni@esi.dz",  // Email to send notification
                            from: "li_louni@esi.dz"  // Sender email
                        )
                    }
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
                    echo 'Build failed. Sending notification via email...'
                    script {
                        sendMail (
                            subject: "Pipeline Failed: ${env.JOB_NAME}",
                            body: """
                                The Jenkins pipeline failed in the stage: Build.

                                Project: ${project.name}
                                Build URL: ${env.BUILD_URL}
                                Please check the logs for more details.
                            """,
                            to: "li_louni@esi.dz",  // Email to send notification
                            from: "li_louni@esi.dz"  // Sender email
                        )
                    }
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
                    echo 'Deployment failed. Sending notification via email...'
                    script {
                        sendMail (
                            subject: "Pipeline Failed: ${env.JOB_NAME}",
                            body: """
                                The Jenkins pipeline failed in the stage: Deploy.

                                Project: ${project.name}
                                Build URL: ${env.BUILD_URL}
                                Please check the logs for more details.
                            """,
                            to: "li_louni@esi.dz",  // Email to send notification
                            from: "li_louni@esi.dz"  // Sender email
                        )
                    }
                }
            }
        }

        stage('Notification') {
            steps {
                echo 'Sending Notification via slack..'
                bat './gradlew postBuiltSucceedToSlack'

                echo 'Sending Notification via email..'
                bat './gradlew sendMail'
            }
            post {
                failure {
                    echo 'Notification failed. Sending notification via email...'
                    script {
                        sendMail (
                            subject: "Pipeline Failed: ${env.JOB_NAME}",
                            body: """
                                The Jenkins pipeline failed in the stage: Notification.

                                Project: ${project.name}
                                Build URL: ${env.BUILD_URL}
                                Please check the logs for more details.
                            """,
                            to: "li_louni@esi.dz",  // Email to send notification
                            from: "li_louni@esi.dz"  // Sender email
                        )
                    }
                }
            }
        }
    }
}
