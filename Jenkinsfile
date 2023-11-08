pipeline {
    agent any

    environment {
        KUBE_NAMESPACE = 'app'
    }
    stages{
        stage("Clone Code"){
            steps{
                sh "sudo apt install git"
                git url: "https://github.com/prasadtara09/node-todo-cicd.git", branch: "master"
            }
        }
        stage("Build and Test"){
            steps{
                sh "docker build . -t node-app-test-new"
            }
        }
        stage("Push to Docker Hub"){
            steps{
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker tag node-app-test-new ${env.dockerHubUser}/node-app-test-new:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/node-app-test-new:latest"
                }
            }
        }
            stage ('EKS Deploy') {
                steps{
                    // Deploy the application to EKS
                    sh "kubectl apply -f your-deployment.yaml -n $KUBE_NAMESPACE"
                }
                
        }
           
    }
    
}
    
