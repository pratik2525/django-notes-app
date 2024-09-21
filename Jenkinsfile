pipeline{
    agent any

    stages{
        stage("Clone the Code"){
            steps{
                echo "Cloning the Code"
                git url:"https://github.com/pratik2525/django-notes-app.git", branch:"main"
            }
        }
        stage("build the code"){
            steps{
                echo "Building the code"
                sh "docker build -t notes-app ."
            }
        }
        stage("push it to dockerhub"){
            steps{
                echo "pushing the image to docker hub"
                withCredentials([usernamePassword(credentialsId: 'dockerHub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]){
                sh "docker tag notes-app $USERNAME/notes-app:latest"
                sh '''
                echo "$PASSWORD" | docker login -u "$USERNAME" --password-stdin
                '''
                sh "docker push $USERNAME/notes-app:latest"
                }
            }
        }
        stage("Deploy"){
            steps{
                echo "Deploying the code"
                sh "docker-compose down && docker-compose up -d"
                echo "Checkout from SCM is tested, Now Webhook is being tested"
            }
        }
    }
}