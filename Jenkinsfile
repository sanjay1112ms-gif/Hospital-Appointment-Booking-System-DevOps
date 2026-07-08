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
                withCredentials([string(credentialsId: 'sonartoken', variable: 'SONAR_TOKEN')]) {
                    sh '''
                    docker run --rm \
                    -v "$PWD":/usr/src \
                    sonarsource/sonar-scanner-cli:latest \
                    -Dsonar.host.url=http://172.17.0.3:9000 \
                    -Dsonar.token=$SONAR_TOKEN \
                    -Dsonar.projectKey=hospital-appointment-booking-system \
                    -Dsonar.projectName=Hospital-Appointment-Booking-System \
                    -Dsonar.sources=.
                    '''
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
