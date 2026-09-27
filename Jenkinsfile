pipeline {
    agent any

    tools {
        go 'Go-set'
    }

    stages {
        stage('Checkout') {
            steps {
                // Если в настройках проекта выбран "Pipeline script from SCM",
                checkout scm
            }
        }
        stage('Test') {
            steps {
                sh 'go test .'
            }
        }
        stage('Build') {
            steps {
                sh 'docker build . -t ubuntu-bionic:8082/hello-world:v$BUILD_NUMBER'
            }
        }
        stage('Push') {
            steps {
                sh '''
                    echo "admin" | docker login ubuntu-bionic:8082 -u admin --password-stdin
                    docker push ubuntu-bionic:8082/hello-world:v$BUILD_NUMBER
                    docker logout ubuntu-bionic:8082
                '''
            }
        }
    }
}
