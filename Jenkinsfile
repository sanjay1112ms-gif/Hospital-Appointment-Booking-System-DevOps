pipeline {
    agent any

    tools {
        nodejs 'Node18'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    def scannerHome = tool 'SonarScanner'

                    withSonarQubeEnv('SonarQube') {
                        withCredentials([string(credentialsId: 'sonartoken', variable: 'SONAR_TOKEN')]) {
                            sh """
                            ${scannerHome}/bin/sonar-scanner \
                            -Dsonar.projectKey=hospital-appointment-booking-system \
                            -Dsonar.projectName=Hospital-Appointment-Booking-System \
                            -Dsonar.sources=. \
                            -Dsonar.token=$SONAR_TOKEN
                            """
                        }
                    }
                }
            }
        }
    }
}
