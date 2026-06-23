pipeline {


agent any

environment {
    IMAGE_NAME = "amanpushp/django-nginx-lab"
}

stages {

    stage('Checkout') {
        steps {
            git branch: 'main',
                url: 'https://github.com/AMANPUSHP23/django-nginx-lab.git'
        }
    }

    stage('Build Docker Image') {
        steps {
            sh 'docker build -t $IMAGE_NAME:latest .'
        }
    }

    stage('Docker Hub Login') {
        steps {
            withCredentials([
                usernamePassword(
                    credentialsId: 'docker-login',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )
            ]) {
                sh '''
                echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                '''
            }
        }
    }

    stage('Push Image') {
        steps {
            sh 'docker push $IMAGE_NAME:latest'
        }
    }

    stage('Deploy to EKS') {
        steps {
            sh '''
            export AWS_PAGER=""
            export KUBECONFIG=/var/jenkins_home/.kube/config

            kubectl get nodes

            kubectl apply -f k8s/

            kubectl rollout restart deployment/django-deployment

            kubectl rollout status deployment/django-deployment --timeout=300s
            '''
        }
    }
}

post {
    success {
        echo 'CI/CD Pipeline Completed Successfully!'
    }

    failure {
        echo 'Pipeline Failed!'
    }
}


}
