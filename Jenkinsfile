pipeline{
    agent any
    stages{
        stage("Git Code Checkout"){
            steps{
                echo "Step For Checkout Source Code fron Github"
                git url: "https://github.com/SomasekharSunkari/django-notes-app",branch : "main" 
            }
        }
        stage("Building the image from it "){
            steps{
                echo "Building  Docker Image "
                sh "docker build -t my-notes-app ."
            }
        }
        stage("Pushing the Image Into Docker Hub"){
            steps{
                echo "Pushing the image into Docker Hub"
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                    sh "docker tag my-notes-app ${env.dockerHubUser}/my-notes-app:latest"
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker push ${env.dockerHubUser}/my-notes-app:latest"
                }
            }
        }
        
        stage("Deploying the Container "){
            steps{
                echo "Deploying the Container"
                
                sh "docker-compose down && docker-compose up "
            }
        }
    }
}
