pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/SyrineOunais/DevOps-AppGestionDesProjets.git'
            }
        }

        stage('Test Docker') {
            steps {
                sh 'docker --version'
                sh 'docker ps'
            }
        }

        stage('Build Backend') {
            steps {
                sh 'docker build -t syrineboh/backend:1.0 ./backend'
            }
        }

        stage('Build Frontend') {
            steps {
                sh 'docker build -t syrineboh/frontend:1.0 ./frontend'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-credentials',
                        usernameVariable: 'DOCKER_USERNAME',
                        passwordVariable: 'DOCKER_PASSWORD'
                    )
                ]) {
                    sh '''
                        echo "$DOCKER_PASSWORD" | docker login \
                            -u "$DOCKER_USERNAME" \
                            --password-stdin

                        docker push syrineboh/backend:1.0
                        docker push syrineboh/frontend:1.0

                        docker logout
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Build et push Docker Hub réussis !'
        }

        failure {
            echo 'Le pipeline a échoué.'
        }
    }
}
