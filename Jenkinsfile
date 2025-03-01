pipeline {
    agent any
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
}
