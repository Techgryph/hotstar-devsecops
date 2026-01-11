pipeline {
    agent any

    environment {
        IMAGE_NAME = "techgryphdocker/hotstar-clone"
        SONAR_PROJECT_KEY = "hotstar-clone"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git url: 'https://github.com/DevOpsInstituteMumbai-wq/Implementing-a-Secure-CI-CD-Pipeline-for-Hotstar-Clone-Using-DevSecOps-Principles.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh '''
                    sonar-scanner \
                    -Dsonar.projectKey=$SONAR_PROJECT_KEY \
                    -Dsonar.sources=src \
                    -Dsonar.host.url=http://localhost:9000 \
                    -Dsonar.login=$SONAR_AUTH_TOKEN
                    '''
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:ci .'
            }
        }

        stage('Docker Scout Scan') {
            steps {
                sh 'docker scout quickview $IMAGE_NAME:ci || true'
            }
        }
    }
}

