pipeline {
    agent any
    
    tools {
        maven 'my_maven'
    }
    
    
    environment {
        GITNAME = 'hjk1996'
        GITMAIL = 'dunhill741@naver.com'
        GITWEBADD = 'https://github.com/hjk1996/aws-test-3.git'
        GITSSHADD = 'git@github.com:hjk1996/spring_deployment.git'
        GITCREDENTIAL = 'github_credential'
        DOCKERHUB = 'dunhill741/spring'
        DOCKERHUBCREDENTIAL = 'docker_credential'
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
                
                withDockerRegistry(credentialsId: DOCKERHUBCREDENTIAL, url: '') {
                    sh "docker push ${DOCKERHUB}:${currentBuild.number}"
                    sh "docker push ${DOCKERHUB}:latest"
                }

                

            
            }
            
            post {
            
                always {
                    sh "docker image rm -f ${DOCKERHUB}:${currentBuild.number}"
                    sh "docker image rm -f ${DOCKERHUB}:latest"
                }
                failure {
                    echo 'docker image push failure'
                }
            
                success {
                    echo 'docker image push success'
                }
        
            }
        
        }
        
        
        stage('k8s manifest file update') {
          steps {
            git credentialsId: GITCREDENTIAL,
                url: GITSSHADD,
                branch: 'main'
            
            // 이미지 태그 변경 후 메인 브랜치에 푸시
            sh "git config --global user.email ${GITEMAIL}"
            sh "git config --global user.name ${GITNAME}"
            sh "sed -i 's@${DOCKERHUB}:.*@${DOCKERHUB}:${currentBuild.number}@g' deployment.yml"
            sh "git add ."
            sh "git commit -m 'fix:${DOCKERHUB} ${currentBuild.number} image versioning'"
            sh "git branch -M main"
            sh "git remote remove origin"
            sh "git remote add origin ${GITSSHADD}"
            sh "git push -u origin main"
    
          }
          post {
            failure {
              echo 'k8s manifest file update failure'
            }
            success {
              echo 'k8s manifest file update success'  
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