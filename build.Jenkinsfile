pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                withCredentials([
                    usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')
                ]) {
                    sh '''
                    docker login --username $USERNAME --password $PASSWORD
                    docker build -t roberta:latest .
                    docker tag roberta:latest niksam20/roberta-cicd:latest
                    docker push niksam20/roberta-cicd:latest
                    '''
                }
            }
            post {
               always {
                 sh '''
                 docker image prune -f -a --filter "until=48h"
                 '''
               }

            }
        }

        stage('Trigger Deploy') {
            steps {
                build job: 'roberta-deploy', wait: false, parameters: [
                    string(name: 'ROBERTA_IMAGE_URL', value: "niksam20/roberta-cicd:latest")
                ]
            }
        }
    }
}
