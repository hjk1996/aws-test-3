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
    }

    stages {
    
        stage('Checkout Github') {
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
                sh -c "mvn clean package"
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