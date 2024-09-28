pipeline {
    agent any

    environment {
        MYSQL_USER = credentials('DB_USER')                     // Reference for MySQL user
        MYSQL_PASSWORD = credentials('DB_PASSWORD')             // Reference for MySQL password
    }
    
    stages{
        stage("Code"){
            steps{
                git url: "https://github.com/ankithaT27/two-tier-flask-app.git", branch: "master"
            }
        }
        stage("Build & Test"){
            steps{
                sh "docker build . -t flaskapp"
            }
        }
        stage("Push to DockerHub"){
            steps{
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker tag flaskapp ${env.dockerHubUser}/flaskapp:${env.BUILD_ID}"
                    sh "docker push ${env.dockerHubUser}/flaskapp:${env.BUILD_ID}" 
                }
            }
        }
        stage("Deploy"){
            steps{
                sh "docker stack deploy -c docker-compose.yml mystack"
            }
        }
        }
    }
}
