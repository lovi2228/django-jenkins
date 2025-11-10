pipeline {
    agent any

    stages {
        stage('Clone Code') {
            steps {
                echo 'cloning the code'
                git url:"https://github.com/lovi2228/django-jenkins.git", branch:"main"
            }
        }
        stage('Build') {
            steps {
                echo 'build the code'
                sh "docker build -t note-app ."
            }
        }
        stage('Pushing the code to Docker Hub') {
            steps {
                echo 'pushing to the Docker'
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass", usernameVariable:"dockerHubUser")]){
                    sh "docker tag note-app ${env.dockerHubUser}/note-app:01"
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker push ${env.dockerHubUser}/note-app:01"
                }
            }
        }
        stage('deploy') {
            steps {
                echo 'Deployed'
                sh '''
                    docker ps -q --filter "publish=8000" | xargs -r docker stop
                    docker ps -aq --filter "publish=8000" | xargs -r docker rm
                    docker run -d -p 8000:8000 lovii28/note-app:01
                    '''

            }
        }
    }
}
