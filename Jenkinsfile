pipeline {
    agent any
    stages {
        stage ('Build docker image'){
            steps {
                script {
                    dockerapp = docker.build("abraao1231/kube-news:${env.BUILD_ID}", '-f ./src/Dockerfile ./src')
                }
            }
        }

        stage ('Push Docker image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub'){
                        dockerapp.push('latest')
                        dockerapp.push("${env.BUILD_ID}")
                }
                }
            }
        }
        stage ('Deploy Kubernets') {
            steps {
                withKubeConfig([credentialsId: "kubeconfig"]){
                    sh 'kubectl apply -f ./k8s/deployment.yaml'
                }
                }
            }
        }
    }

