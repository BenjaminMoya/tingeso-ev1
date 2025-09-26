pipeline {
    agent any
    tools{
        gradle "gradle"
    }
    stages{
        stage("Build .jar"){
            steps{
                checkout scmGit(branches: [[name: '*/main']],extensions: [], userRemoteConfigs: [[url: 'https://github.com/BenjaminMoya/tingeso-ev1']])
                dir("CreditAdministrationApplication"){
                    sh "gradle clean build"
                }
            }
        }
        stage("Build and Push Docker Image"){
            steps {
                dir("CreditAdministrationApplication"){
                    withCredentials([usernamePassword(credentialsId: 'credentialsId', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                        sh "docker login -u $USER -p $PASS"
                        sh "docker build --no-cache -t benjaminmoya/creditapp-backend:latest ."
                        sh "docker push benjaminmoya/creditapp-backend:latest"
                    }
                }
            }
        }
    }
}