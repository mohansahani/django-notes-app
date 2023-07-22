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
                echo "Building the Image"
                sh "docker build -t my-note-app ."
            }
        }
        stage("Push to Docker Hub"){
            steps{
                echo "Pushing the Image to Docker hub"
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){ 
                sh "docker tag my-note-app ${env.dockerHubUser}/my-note-app:latestV1"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/my-note-app:latestV1"
            }
                
            }
        }
        stage("Deploy"){
            steps{
                echo "Deploying the Code"
                sh "docker-compose down && docker-compose up -d" 
                
            }
        }
    }
}
