pipeline {
    agent any

    environment {
        IMAGE_NAME = 'jaymeenk/scientific-calculator'
        IMAGE_TAG = 'latest'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Clean Old Docker Image') {
            steps {
                script {
                    def imageExists = sh(script: "docker images -q ${IMAGE_NAME}:${IMAGE_TAG}", returnStdout: true).trim()
                    if (imageExists) {
                        echo "Deleting existing Docker image: ${IMAGE_NAME}:${IMAGE_TAG}"
                        sh "docker rmi -f ${IMAGE_NAME}:${IMAGE_TAG}"
                    } else {
                        echo "No existing Docker image found for ${IMAGE_NAME}:${IMAGE_TAG}"
                    }
                }
            }
        }
        stage('Docker Build') {
            steps {
                sh 'docker build -t jaymeenk/scientific-calculator:latest .'
            }
        }
        stage('Docker Push') {
            steps {
                withDockerRegistry([credentialsId: '545029ae-9452-4e35-a1b2-02469129502d', url: 'https://index.docker.io/v1/']) {
                    sh 'docker push jaymeenk/scientific-calculator:latest'
                }
            }
        }
    }
    post {
        always {
            junit '**/target/surefire-reports/*.xml'
        }
    }
}
