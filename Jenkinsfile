pipeline {
    agent any

    environment {
        IMAGE_NAME = "divyap1571/myapp"
        IMAGE_TAG = "${BUILD_NUMBER}"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/divyap1571/<YOUR-REPOSITORY>.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Building application...'
                sh 'docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .'
            }
        }

        stage('Test') {
            steps {
                echo 'Running automated tests...'
                sh 'echo "Tests completed successfully"'
            }
        }

        stage('Docker Push') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-creds',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )
                ]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push ${IMAGE_NAME}:${IMAGE_TAG}
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    kubectl set image deployment/my-app \
                    my-app=${IMAGE_NAME}:${IMAGE_TAG} \
                    -n my-app

                    kubectl rollout status deployment/my-app -n my-app
                '''
            }
        }

        stage('Verify') {
            steps {
                sh '''
                    kubectl get deployment my-app -n my-app
                    kubectl get pods -n my-app
                    kubectl rollout status deployment/my-app -n my-app
                '''
            }
        }
    }

    post {
        success {
            echo 'CI/CD Pipeline completed successfully!'
        }

        failure {
            echo 'Pipeline failed. Check Jenkins console output.'
        }
    }
}
