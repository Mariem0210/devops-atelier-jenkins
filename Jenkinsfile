pipeline {
    agent any

    environment {
        DOCKER_USER = 'mariemchouchene'
        TAG         = "${BUILD_NUMBER}"
    }

    stages {
        stage('Checkout') {
            steps { checkout scm }
        }

        stage('Docker Build') {
            steps { sh 'docker compose build' }
        }

        stage('Docker Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub',
                        usernameVariable: 'DH_USER', passwordVariable: 'DH_PASS')]) {
                    sh '''
                        echo "$DH_PASS" | docker login -u "$DH_USER" --password-stdin
                        docker compose push
                        docker tag $DOCKER_USER/gestion-backend:$TAG  $DOCKER_USER/gestion-backend:latest
                        docker tag $DOCKER_USER/gestion-frontend:$TAG $DOCKER_USER/gestion-frontend:latest
                        docker push $DOCKER_USER/gestion-backend:latest
                        docker push $DOCKER_USER/gestion-frontend:latest
                    '''
                }
            }
        }

        stage('Deploy') {
            steps { sh 'docker compose up -d' }
        }
    }

    post {
        always { sh 'docker logout || true' }
    }
}
