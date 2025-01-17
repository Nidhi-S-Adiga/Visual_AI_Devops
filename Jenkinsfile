pipeline {
    agent any

    stages {
        stage('Pull Docker Image') {
            steps {
                script {
                    sh 'docker pull urmsandeep/ai-artistic-style-service:latest'
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    sh '''
                    docker run --rm -p 5001:5001 urmsandeep/ai-artistic-style-service:latest pytest tests/
                    '''
                }
            }
        }

        stage('Deploy Service') {
            steps {
                script {
                    sh '''
                    docker-compose down
                    docker-compose up -d
                    '''
                }
            }
        }

        stage('Verify Deployment') {
            steps {
                script {
                    sh '''
                    sleep 5
                    curl -X POST http://127.0.0.1:5001/styleTransfer -F "image=@test.jpg" --output styled_output.jpg
                    '''
                }
            }
        }
    }

    post {
        always {
            sh 'docker system prune -f'
        }
        success {
            echo 'Pipeline executed successfully. The service is running and functional!'
        }
        failure {
            echo 'Pipeline failed. Check logs for errors.'
        }
    }
}
