pipeline {
    agent { label "mehul"}
    
    stages {
        stage("Code"){
            steps{
                echo "This is cloning the code"
                git url: "https://github.com/mehulmorker/jenkins-demo-code.git", branch:"main"
                echo "Code cloning successful"
            }
        }
        stage("Build"){
            steps{
                echo "This is building the code"
                sh "docker build -t node-jenkins-demo:latest ."
            }
        }
        stage("Test"){
            steps{
                echo "This is testing the code"
            }
        }
        
        stage("Push to DockerHub"){
            steps{
                echo "This is pushing image to docker hub"
                withCredentials([usernamePassword(
                    'credentialsId': 'dockerHubCred', 
                    passwordVariable: "dockerHubPass", 
                    usernameVariable: "dockerHubUser")]){
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker image tag node-jenkins-demo:latest ${env.dockerHubUser}/node-jenkins-demo:latest"
                sh "docker push ${env.dockerHubUser}/node-jenkins-demo:latest"
             }
            }
        }
        stage("Deploy"){
            steps{
                echo "This is deploying the code"
                sh "docker compose up -d"
            }
        }
    }
}
