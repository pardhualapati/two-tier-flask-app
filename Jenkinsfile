@Library("shared") _
pipeline{
    agent {label "dev" };
    stages{
        stage("Code"){
            steps{
                script{
                    clone("https://github.com/pardhualapati/two-tier-flask-app.git/","dockercompose")
                }
            }
        }
        stage("Trivy File System Scan"){
            steps{
                trivy_fs()
            }
        }
        stage("Build"){
            steps{
                sh "docker build -t 2tierapp ."
                echo "Building the  Code!!"
            }
        }
        stage("Testing"){
            steps{
                echo "Testing is done by Tester so it is done by a tester"
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

post{
        success{
            script{
                emailext from: "pardhualapati01@gmail.com",
                to: "pardhu.alapati56@gmail.com",
                body: "Good News: Your Build was Successfull!",
                subject: "Build Successfull"
               
            }
            
        }
        failure{
            script{
                emailext from: "pardhualapati01@gmail.com",
                to: "pardhualapati56@gmail.com",
                body: "OOPS build Failed",
                subject: "Build Failed"
                
            }
        }
    
    
}

}
