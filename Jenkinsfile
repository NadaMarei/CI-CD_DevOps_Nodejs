pipeline {
    agent any
    environment {
        AWS_ACCESS_KEY_ID = credentials('AWS_ACCESS_KEY_ID')
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
        AWS_DEFAULT_REGION = "us-east-1"
    }

    stages {
        stage('CI') {
            steps {
                
                
                
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                git 'https://github.com/NadaMarei/DevOps_Project_NodejsApp.git'
                sh """
                docker login -u ${USERNAME} -p ${PASSWORD}
                docker build . -f dockerfile -t nadamarey/devops-image:v1.0 --network host

                docker push nadamarey/devops-image:v1.0

               
                """
                }
            }
        }
         stage('CD') {
            steps {
                git 'https://github.com/NadaMarei/DevOps_Project_NodejsApp.git'
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                sh """
                kubectl create namespace app
             
                kubectl apply -f /var/jenkins_home/workspace/DevOps-project-iti/deployment-app.yaml -n app

                kubectl apply -f /var/jenkins_home/workspace/DevOps-project-iti/service-app.yaml -n app

                """
                }
            }
        }
    }
}
