pipeline {
    agent any
    stages{
        stage("Clone Code"){
            steps{
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
            stage ('Kubernetes Deploy') {
                kubernetesDeploy(
                configs: 'node-todo-cicd/Deployment.yaml',
                kubeconfigId: 'K8S',
                enableConfigSubstitution: true
                )
        }
           
    }
    
}
    
