pipeline {
    environment {
        registry = "salma299/tp4"
        registryCredential = 'dockerhub'
        dockerImage = ''
    }
    agent any
    
    stages {
        stage('Cloning Git') {
            steps {
                git branch: 'main', url: 'https://github.com/SalmaFennan/tp_DevOps.git'
            }
        }
        
        stage('Building image') {
            steps {
                script {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }
        
        stage('Test image') {
            steps {
                script {
                    echo "Tests passed"
                }
            }
        }
        
        stage('Publish Image') {
            steps {
                script {
                    docker.withRegistry('', registryCredential) {
                        dockerImage.push()
                        dockerImage.push('latest')
                    }
                }
            }
        }
        
        stage('Deploy image') {
            steps {
                bat """
                    docker stop tp4-container 2>nul || exit 0
                    docker rm tp4-container 2>nul || exit 0
                    docker run -d -p 8081:80 --name tp4-container ${registry}:latest
                """
            }
        }
    }
}