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
        stage('Build') {
            steps {
                echo 'Building..'
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