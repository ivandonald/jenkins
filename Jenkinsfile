pipeline{
    agent {label "jenkins-agent"}
    stages{
        stage("Clone Code"){
            steps{
                git url: "https://github.com/ivandonald/jenkins.git", branch: "main"
            }
        }
        stage("Build and Test") {
            steps{
                sh "docker build . -t django-todo-app"
            }
        }
        stage("Login and Push image")
        {
            steps{
                withCredentials([usernamePassword(credentialsId:"dockerhub",passwordVariable:"DOCKER_PASSWORD",usernameVariable:"DOCKER_USERNAME")]){
                    sh """
                    #!/bin/sh
                    docker tag django-todo-app $DOCKER_USERNAME/django-todo-app:latest
                    docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD
                    docker push $DOCKER_USERNAME/django-todo-app:latest
                    """
                }
            }
        }
        stage("Deploy") {
            steps{
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
