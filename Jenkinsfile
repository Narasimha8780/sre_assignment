pipeline {
    agent any  
      stages {
        stage('Clone Repo') {
          steps {
            sh 'rm -rf jenkinstest'
            sh 'git clone https://github.com/Narasimha8780/jenkinstest.git'
            }
        }

        stage('Build Docker Image') {
            steps {
              sh 'docker build -t narasimha8780/jenkinstest:latest .'
              }
        }
        stage('Push Docker image') {
            environment {
                DOCKER_HUB_LOGIN = credentials('docker')
            }
            steps {
                sh 'docker login --username=$DOCKER_HUB_LOGIN_USR --password=$DOCKER_HUB_LOGIN_PSW'
                sh    'docker push narasimha8780/jenkinstest:latest'
            }
        }
          
        stage('K8S Deploy') {
            steps {
                withKubeConfig([credentialsId: 'k8s', serverUrl: '']) {
                    sh ('kubectl apply -f  kube.yaml')
                }
            }
        }
        
        
      }
}