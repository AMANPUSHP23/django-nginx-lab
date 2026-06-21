pipeline {


agent any

stages {

    stage('Checkout') {
        steps {
            git branch: 'main',
                url: 'https://github.com/AMANPUSHP23/django-nginx-lab.git'
        }
    }

    stage('Deploy to App Server') {
        steps {

            sshagent(['app-server-ssh']) {

                sh '''
                ssh -o StrictHostKeyChecking=no ubuntu@18.233.160.163 "

                cd ~/django-nginx-lab &&

                git pull origin main &&

                docker compose down &&

                docker compose up -d --build

                "
                '''

            }
        }
    }

}


}

