pipeline{
    agent any

    stages{

        stage('Builder Docker Image'){
            steps{
                script{
                    dockerapp = docker.build("celsojrss96/kube-news:${env.BUILD_ID}", '-f ./src/Dockerfile ./src')
                }
            }
        }
    }
}