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
                echo 'Testing..'
                bat './gradlew test'
            }
        }
    }
}