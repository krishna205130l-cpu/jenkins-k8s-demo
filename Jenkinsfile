pipeline {
    agent any

    options {
        disableConcurrentBuilds()
        timeout(time: 5, unit: 'MINUTES')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Deploy') {
            steps {
                withKubeConfig([credentialsId: 'k8s-kubeconfig']) {
                    sh '''
                        set -eu
                        kubectl -n jenkins-demo apply -f k8s/app.yaml
                        kubectl -n jenkins-demo rollout status deployment/simple-web --timeout=120s
                    '''
                }
            }
        }

        stage('Verify') {
            steps {
                withKubeConfig([credentialsId: 'k8s-kubeconfig']) {
                    sh '''
                        kubectl -n jenkins-demo get deployments,pods,services
                    '''
                }
            }
        }
    }
}
