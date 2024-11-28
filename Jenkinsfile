pipeline{
    agent any
    
    stages{
        stage("Code clone"){
            steps{
                echo "Cloning the repo done"
                git url:"https://github.com/parth6768/node-todo-project-jenkins-1.git", branch:"master"
                echo "Repository cloned successfully."
            }
        }
        stage("Image Build"){
            steps{
                echo "Building Docker image..."
                sh "docker build -t node-app ."
                echo "Docker image built successfully."
            }
        }
        stage("pushimagedockerhub"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId:"dockerHubCreds",
                    usernameVariable:"dockerHubUser",
                    passwordVariable:"dockerHubPass")]){
                        sh "echo $dockerHubPass | docker login -u $dockerHubUser --password-stdin"
                        sh "docker image tag node-app:latest ${env.dockerHubUser}/node-app:latest"
                        sh "docker push ${env.dockerHubUser}/node-app:latest"
                    }
            }
        }
        stage("Deploy"){
            steps{
                echo "Deploying application..."
                sh "docker compose down && docker compose up -d --build"
                echo "Application deployed successfully."
            }
        }
    }
}

