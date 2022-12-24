pipeline{
    agent any
    environment{
        registry = "109722177373.dkr.ecr.us-east-1.amazonaws.com/myecrrepo"
    }
    stages{
        stage("Checkout"){
            steps{
            checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/eswari795/django-todo.git']]])

            }
        }

        stage("Build the image")
        {
            steps{
            script{
                docker.build registry
            }
            }
            
        }

        stage("push to ecr"){
            steps{
                sh "aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 109722177373.dkr.ecr.us-east-1.amazonaws.com"
                sh "docker push 109722177373.dkr.ecr.us-east-1.amazonaws.com/myecrrepo:latest"
            }
        }

        stage("deploy to k8"){
            steps{
                withKubeConfig(caCertificate: '', clusterName: '', contextName: '', credentialsId: 'k8s', namespace: '', serverUrl: '') {
                        sh "kubectl create -f k8manifest.yaml"
                }
            }
        }

    }
}