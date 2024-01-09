pipeline {
    agent any
    
    tools {
        maven 'my_maven'
    }
    
    
    environment {
        GITNAME = 'hjk1996'
        GITMAIL = 'dunhill741@naver.com'
        GITWEBADD = 'https://github.com/hjk1996/aws-test-3.git'
        GITSSHADD = 'git@github.com:hjk1996/aws-test-3.git'
        GITCREDENTIAL = 'github_credential'
        DOCKERHUB = 'dunhill741/spring'
        DOCKERHUBCREDENTIAL = 'dockerhub_credential'
    }
    stages {
    
        stage('checkout github') {
            steps {
                echo "Checkout Github..."
            
            
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [],
                userRemoteConfigs: [[credentialsId: GITCREDENTIAL, url: GITWEBADD]]])
            
            
            }
            
            post {
                failure {
                    echo 'repository clone failure'
                }
            
                success {
                    echo 'repository clonse success'
                }
        
            }
        
        }
        

    
        stage('code build') {
            steps {
                echo 'Building..'
                sh "mvn clean package"
            }
            
            
        }
        
        stage('docker image build') {
            steps {
                echo 'build docker image...'
                // currentBuild.number = 젠킨스의 빌드 넘버
                sh "docker build -t ${DOCKERHUB}:${currentBuild.number} ."
                sh "docker tag ${DOCKERHUB}:${currentBuild.number} ${DOCKERHUB}:latest"
            
            }
            
            
            post {
                failure {
                    echo 'docker image build failure'
                }
            
                success {
                    echo 'docker image build success'
                }
        
            }
        
        }
        
        stage('docker image push') {
            steps {
                echo 'pushing docker image...'
                sh "docker push ${DOCKERHUB}:${currentBuild.number}"
                sh "docker push ${DOCKERHUB}:latest"

            
            }
            
            post {
                failure {
                    echo 'docker image push failure'
                    sh "docker image rm -f ${DOCKERHUB}:${currentBuild.number}"
                    sh "docker image rm -f ${DOCKERHUB}:latest"
                    
                }
            
                success {
                    echo 'docker image push success'
                    sh "docker image rm -f ${DOCKERHUB}:${currentBuild.number}"
                    sh "docker image rm -f ${DOCKERHUB}:latest"
                }
        
            }
        
        }
        
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}