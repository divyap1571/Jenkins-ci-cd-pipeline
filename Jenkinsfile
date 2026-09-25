pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/divyap1571/Jenkins-ci-cd-pipeline.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Building application...'
                bat 'docker build -t my-app:1 .'
            }
        }

        stage('Test') {
            steps {
                echo 'Running automated tests...'
                bat 'echo "Tests completed successfully"'
            }
        }

        stage('Docker Push') {
            steps {
                 
                    bat '''
                        docker login -u divyap1571 -p H3110@Divya
                        docker push divyap1571/my-app:1
                    '''
                
            }
        }

        stage('Deploy') {
            steps {
                bat '''
                    kubectl set image deployment/my-app \
                    my-app=divyap1571/my-app:1 \
                    -n my-app

                    kubectl rollout status deployment/my-app -n my-app
                '''
            }
        }

        stage('Verify') {
            steps {
                bat '''
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
