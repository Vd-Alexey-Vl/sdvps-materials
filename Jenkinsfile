pipeline {
    agent any

    tools {
        go 'Go-set'  
    }

    stages {
        stage('Test') {
            steps {
                sh 'go test .'
            }
        }
        stage('Build Binary') {
            steps {
                sh 'go build -o hello-world .'
            }
        }
        stage('Upload to Nexus') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'nexus-cred',
                    usernameVariable: 'NEXUS_USER',
                    passwordVariable: 'NEXUS_PASS'
                )]) {
                    sh '''
                        curl -u $NEXUS_USER:$NEXUS_PASS \
                            --upload-file hello-world \
                            http://127.0.0.1:8081/repository/go-binaries/hello-world-v$BUILD_NUMBER
                    '''
                }
            }
        }
    }
}
