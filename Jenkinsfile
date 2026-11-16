pipeline {
    options { timestamps() }

    agent none
    stages {
        stage('Check scm') {
            agent any
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                echo "Building ...${BUILD_NUMBER}"
                echo "Build completed"
            }
        }
        stage('Test') {
            agent { 
                docker { 
                    image 'alpine'
                    args '-u=root'
                }
            }
            steps {
                sh 'apk add --update python3 py-pip'
                sh 'pip install --break-system-packages Flask'
                sh 'pip install --break-system-packages unittest-xml-reporting'
                sh 'python3 app_tests.py'
            }
            post {
                always {
                    junit 'test-reports/*.xml'
                }
                success {
                    echo "Application testing successfully completed "
                }
                failure {
                    echo "Oooppsss!!! Tests failed!"
                }
            }
        }
        stage('Docker Build & Push') {
            steps {
                script {
                    app = docker.build("sabdshh/python-flask-app:${BUILD_NUMBER}", "-f Task/Dockerfile .")
                    
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials') {
                        app.push()
                        app.push("latest")
                    }
                }
            }
        }
    }
}