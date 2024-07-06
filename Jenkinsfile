pipeline {
    agent any
    tools{
        maven 'maven3'
    }
    
    environment {
        DOCKERHUB_CRED = credentials('docker-hub') // Assuming dockerhub-pwd is the credential ID
    }
    stages{
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/abhisek14/devops-automation.git']]])
                bat 'mvn -f pom.xml clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    bat 'docker build -t abhisek147/devops-integration .'
                }
            }
        }
stage('Push image to Hub'){
            steps{
                script{
                   withDockerRegistry(credentialsId: 'docker-hub', url: 'https://index.docker.io/v1/') {
  



                   bat 'docker push abhisek147/devops-integration'
                }
            }
        }
}
       
        stage('Deploy to k8s'){
            steps{
                script{
                    withKubeConfig(caCertificate: '', clusterName: 'minikube', contextName: '', credentialsId: 'K8S', namespace: '', restrictKubeConfigAccess: false, serverUrl: '') {
    bat 'kubectl apply -f deploymentservice.yaml'

                }
            }
        }
    }
}
}
