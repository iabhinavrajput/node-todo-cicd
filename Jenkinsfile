pipeline {
    agent { label "dev-server" }
    stages {
        stage("code clone from the github...") {
            steps {
                git url : "https://github.com/iabhinavrajput/node-todo-cicd.git", branch : "master"
            }
        } 
        stage("code building...") {
            steps {
                echo "the image is creating..."
                sh "docker build -t todo-app ."
            }
        }
        stage("push to dockerHub") {
            steps {
                withCredentials([usernamePassword(
                    credentialsId:"dockerHubCreds", 
                    usernameVariable:"dockerHubUser", 
                    passwordVariable:"dockerHubPass")]) {
                    
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker image tag todo-app:latest ${env.dockerHubUser}/node-app:latest"
                    sh "docker push ${env.dockerHubUser}/node-app:latest"
                    }
            }
        }
        stage("code deploying....") {
            steps {
                echo "the code is deploying"
                sh ''' docker compose down || true 
                    docker compose up -d --build
                '''
            }
        }
    }
}
