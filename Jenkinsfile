pipeline {
    agent any
    
    stages{
        stage("Code"){
            steps{
                echo "Cloning the Code"
                git url:"https://github.com/mohansahani/django-notes-app.git", branch: "main"
            }
        }
        stage("Build"){
            steps{
                echo "Building the image"
                sh "docker build -t note-app ."
            }
        }
        stage("Push to docker hub"){
            steps{
                echo "Pushing the image to docker hub"
                withCredentials([usernamePassword(credentialsId:"dockerhub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker tag note-app ${env.dockerHubUser}/note-app:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/note-app:latest"
            }
            }
        }
        stage("Deploy"){
            steps{
                echo "Deplying the container"
                sh "docker-compose down && docker-compose up -d "
            }
        }
    } 
}
