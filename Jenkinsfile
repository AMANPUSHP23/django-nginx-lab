pipeline {

```
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

    stage('Deploy') {
        steps {
            sshagent(['app-server-ssh']) {
                sh '''
                ssh -o StrictHostKeyChecking=no ubuntu@18.233.160.163 "
                cd ~/django-nginx-lab &&
                docker pull amanpushp/django-nginx-lab:latest &&
                docker compose down &&
                docker compose up -d
                "
                '''
            }
        }
    }
}
```

}

