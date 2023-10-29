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
        }
    }
}
