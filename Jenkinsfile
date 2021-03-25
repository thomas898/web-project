def dev = [:]
dev.name = 'ubuntu'
dev.host = '65.1.253.61'
dev.allowAnyHosts = true
pipeline {
    agent any
    environment{
       REPOSITORY_URI="accountid.dkr.ecr.ap-south-1.amazonaws.com/nginx-custom"
}
   
    stages {
        stage('Checkout SCM ') {
             steps {
              git branch: 'main', url: 'https://github.com/thomas898/web-project'
             
            }
        }
         stage('docker build ') {
             steps {
              sh '''
              docker build -t $REPOSITORY_URI:latest .
              '''
             
            }
        }
        stage('docker push ') {
             steps {
              sh '''
              eval $(aws ecr get-login --no-include-email --region ap-south-1)
              docker push $REPOSITORY_URI:latest
              '''
              }
        }
              stage("deploy") {
        steps {
          script {
            withCredentials([sshUserPrivateKey(credentialsId: 'nginx-deployment', keyFileVariable: 'tldev', passphraseVariable: '', usernameVariable: 'ubuntu')]) {
            dev.user = ubuntu
            dev.identityFile = tldev
                writeFile file: 'devupdate.sh', text: '#!/bin/bash \n \n set -e \n sudo su \n cd /home/ubuntu/ \n eval $(aws ecr get-login --no-include-email --region ap-south-1) \n docker stop nginx \n docker rm $(docker ps -aq) \n docker pull 844583223092.dkr.ecr.ap-south-1.amazonaws.com/nginx-custom \ndocker run --name nginx -d -p 80:80 844583223092.dkr.ecr.ap-south-1.amazonaws.com/nginx-custom'
                 sshScript remote: dev, script: "devupdate.sh"
              }
          }
        }
        }
    
    }
}
