pipeline {
    agent any

    environment {
        IMAGE_NAME = "techgryphdocker/hotstar-clone"
        SONAR_PROJECT_KEY = "hotstar-clone"
    }

    stages {

<<<<<<< HEAD
=======
        stage('Checkout Code') {
            steps {
                git url: 'https://github.com/https://github.com/Techgryph/hotstar-devsecops.git'
            }
        }

>>>>>>> c3bb3ec (Add Docker push stage)
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
                sh '''
                docker build -t $IMAGE_NAME:$BUILD_NUMBER .
                docker tag $IMAGE_NAME:$BUILD_NUMBER $IMAGE_NAME:latest
                '''
            }
        }

        stage('Docker Scout Scan') {
            steps {
                sh 'docker scout quickview $IMAGE_NAME:$BUILD_NUMBER || true'
            }
        }

        stage('Push Image to DockerHub') {
            steps {
                withDockerRegistry([credentialsId: 'dockerhub-creds']) {
                    sh '''
                    docker push $IMAGE_NAME:$BUILD_NUMBER
                    docker push $IMAGE_NAME:latest
                    '''
                }
            }
        }
    }
}

