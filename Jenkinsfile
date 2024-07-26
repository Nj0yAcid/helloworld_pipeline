pipeline {
    agent any
    tools{
        maven 'M2_HOME'
    }
    environment {
        registry = '211125431540.dkr.ecr.us-east-1.amazonaws.com/devop_repository'
        registryCredential = 'jenkins-ecr'
        dockerimage = ''
    }
    stages {
        stage('Checkout'){
            steps{
                git branch: 'main', url: 'https://github.com/Nj0yAcid/helloworld_pipeline.git'
            }
        }
        stage('Code Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Build Image') {
            steps {
                script{
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                } 
            }
        }
        stage('Deploy image') {
            steps{
                script{ 
                    docker.withRegistry("https://"+registry,"ecr:us-east-1:"+registryCredential) {
                        dockerImage.push()
                    }
                }
            }
        } 

        stage('docker login'){
            steps {
               sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 211125431540.dkr.ecr.us-east-1.amazonaws.com' 
            }
        } 
    }
}
