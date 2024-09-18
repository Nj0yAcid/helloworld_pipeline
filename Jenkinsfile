pipeline{
    agent any
    
    tools{
        maven 'M2_HOME'
    }

    environment{
        registry = '211125431540.dkr.ecr.us-east-1.amazonaws.com/devops_repository'
        registryCredential = 'jenkins-ecr'
        dockerimage = ''
    }

    stages{
        stage('Checkout'){
            steps{
                git branch: 'main', url: 'https://github.com/Nj0yAcid/helloworld_pipeline.git'
            }
        }
    

        stage('clean'){
            steps{
                sh 'mvn clean '
                sh 'mvn validate'
                sh 'mvn compile' 
                sh 'mvn install' 
                sh 'mvn test'
            }
        }

        stage('validate'){
            steps{
                sh 'mvn validate'
                sh 'mvn compile' 
                sh 'mvn install' 
                sh 'mvn test'
            }
        }
        
        stage('compile'){
            steps{
                sh 'mvn compile' 
                sh 'mvn test'
            }
        }

        stage('test'){
            steps{
                sh 'mvn test'
            }
        }

        stage('Build'){
            steps{
                sh 'mvn package'
            }
        }

        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t ${registry}:{BUILD_ID} .'
                }
            }
        }

        stage('Push docker image in ECR'){
            steps{
                script{
                    sh 'docker push ${registry}:{BUILD_ID}'
                }
            }
        }
    }
}

