pipeline {
    agent any

    environment {
        IMAGE_NAME = "techgryphdocker/hotstar-clone"
        SONAR_PROJECT_KEY = "hotstar-clone"
    }

    stages {

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

stage('SonarQube Analysis') {
    steps {
        withSonarQubeEnv('sonarqube') {
            sh """
            ${tool 'SonarScanner'}/bin/sonar-scanner \
            -Dsonar.projectKey=hotstar-clone \
            -Dsonar.sources=src
            """
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

