pipeline{
    agent any;
    stages{
        stage("Code"){
            steps{
                git url: "https://github.com/pardhualapati/two-tier-flask-app.git", branch:"dockercompose"
                echo " Code Copied!"
            }
        }
        stage("Build"){
            steps{
                sh "docker build -t 2tierapp ."
                echo "Building the Code"
            }
        }
        stage("Testing"){
            steps{
                echo "Testing is done by Tester"
            }
        }
        stage("Push to DockerHub"){
            
            steps{
                withCredentials([usernamePassword(
                    credentialsId:"dockerHubCreds",
                    passwordVariable: "dockerHubpass",
                    usernameVariable: "dockerHubuser"
                )]){
                    
                    sh "docker login -u ${env.dockerHubuser} -p ${env.dockerHubpass}"
                    sh "docker image tag 2tierapp ${env.dockerHubuser}/2tierapp"
                    sh "docker push ${env.dockerHubuser}/2tierapp"
                }
            }
        }
        stage("Deploy"){
            steps{
                sh "docker compose up -d --build flask-app"
                echo "Deployment Successfull"
            }
        }
        
        
    }
}
