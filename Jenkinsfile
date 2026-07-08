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

        stage('Backend Install') {
            steps {
                dir('backend') {
                    sh 'npm install'
                }
            }
        }

        stage('Frontend Install') {
            steps {
                dir('frontend') {
                    sh 'npm install --legacy-peer-deps'
                }
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
                                -Dsonar.token=\$SONAR_TOKEN
                            """

                        }
                    }
                }
            }
        }

        stage('Build Docker Images') {
            steps {
                sh 'docker build -t sanjay1112ms/hospital-backend:v1 ./backend'
                sh 'docker build -t sanjay1112ms/hospital-frontend:v1 ./frontend'
            }
        }

        stage('Docker Images') {
            steps {
                sh 'docker images'
            }
        }
    }
}
