pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'neilsamaa/demo-app'
        DOCKER_TAG = 'latest'
        DOCKER_CREDENTIALS_ID = 'dockerhub-credentials'
        HELM_CHART_DIR = 'helm-chart'
        K8S_NAMESPACE = 'default'
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}:${DOCKER_TAG}")
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', "${DOCKER_CREDENTIALS_ID}") {
                        docker.image("${DOCKER_IMAGE}:${DOCKER_TAG}").push()
                    }
                }
            }
        }

        stage('Deploy to Minikube') {
            steps {
                script {
                    sh "helm install demo-app ${HELM_CHART_DIR}"
                }
            }
        }

        stage('Verify Deployment') {
            steps {
                script {
                    sh "kubectl get pods --namespace ${K8S_NAMESPACE}"
                }
            }
        }

        stage('Port Forward') {
            steps {
                script {
                    sh "kubectl port-forward svc/demo-app 5000:5000"
                }
            }
        }

        stage('Test Application') {
            steps {
                script {
                    sh "curl -vvv http://localhost:5000 || exit 1"
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
