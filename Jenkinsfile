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

        stage('Push Docker Image'){
            steps{
                script{
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub'){
                        dockerapp.push('lateste')
                        dockerapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }

        stage('Deploy Kubernetes'){
            environment{
                tag_version = "${env.BUILD_ID}"
            }
            steps{
                withKubeConfig([credentialsId: 'kubeconfig']){
                    sh 'sed -i "s/{{TAG}}/$tag_version/g" ./k8s/deployment.yaml'
                    sh 'kubectl apply -f ./k8s/deployment.yaml'
                }
            }
        }
    }
}