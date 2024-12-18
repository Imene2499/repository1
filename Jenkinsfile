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
                            bat './gradlew build'

                echo 'Testing..'
                bat './gradlew check'
            }
        }
    }
}