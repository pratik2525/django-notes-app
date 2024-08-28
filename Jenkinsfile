pipeline {
        agent any
        
        stages{
            stage("Clone the code"){
                steps{
                    echo "Cloning the code"
                    git url:"https://github.com/pratik2525/django-notes-app", branch: "main"
                }
            }
            stage("Build the code"){
                steps{
                    echo " building the code"
                    sh "docker build -t notes-app ."
                }
            }
            stage("Push it to Docker Hub"){
                steps{
                    echo "pushing the image to Docker Hub"
                    withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                    sh "docker tag notes-app $dockerHubUser/notes-app:latest"
                    sh '''
                    echo "$dockerHubPass" | docker login -u "$dockerHubUser" --password-stdin
                    '''
                    sh "docker push $dockerHubUser/notes-app:latest"
                    }
                }
            }
            stage("Deploy"){
                steps{
                    echo "Deploy the image to container"
                    sh "docker-compose down && docker-compose up -d"

                }
            }
        }

}
