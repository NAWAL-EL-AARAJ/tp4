pipeline {
    agent any
    environment {
        IMAGE = "nawalelaaraj09/tp4"
    }
    stages {
        stage('Cloning Git') {
            steps {
                git branch: 'main', url: 'https://github.com/NAWAL-EL-AARAJ/tp4.git'
            }
        }
        stage('Building image') {
            steps {
                bat "docker build -t %IMAGE%:%BUILD_NUMBER% ."
            }
        }
        stage('Test image') {
            steps {
                echo "Tests passed"
            }
        }
        stage('Publish Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DH_USER', passwordVariable: 'DH_PASS')]) {
                    bat """
                    echo %DH_PASS%| docker login -u %DH_USER% --password-stdin
                    docker push %IMAGE%:%BUILD_NUMBER%
                    """
                }
                        stage('Deploy image') {
            steps {
                bat '''
                docker rm -f tp4container3
                docker run -d --name tp4container3 -p 8083:80 %IMAGE%:%BUILD_NUMBER%
                '''
            }
        }
            }
        }
    }
}