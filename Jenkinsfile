pipeline {
    agent any

    environment {
        IMAGE_NAME = "techgryphdocker/hotstar-clone"
        SONAR_PROJECT_KEY = "hotstar-clone"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Techgryph/hotstar-devsecops.git'
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
                    withCredentials([
                        string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')
                    ]) {
                        sh """
                        sonar-scanner \
                        -Dsonar.projectKey=${SONAR_PROJECT_KEY} \
                        -Dsonar.sources=src \
                        -Dsonar.host.url=http://localhost:9000 \
                        -Dsonar.login=${SONAR_TOKEN}
                        """
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh """
                docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} .
                docker tag ${IMAGE_NAME}:${BUILD_NUMBER} ${IMAGE_NAME}:latest
                """
            }
        }

        stage('Docker Scout Scan') {
            steps {
                sh "docker scout quickview ${IMAGE_NAME}:${BUILD_NUMBER} || true"
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry(
                        [url: 'https://index.docker.io/v1/', credentialsId: 'dockerhub-creds']
                    ) {
                        sh """
                        docker push ${IMAGE_NAME}:${BUILD_NUMBER}
                        docker push ${IMAGE_NAME}:latest
                        """
                    }
                }
            }
        }
    }
}



