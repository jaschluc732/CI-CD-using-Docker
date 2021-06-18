pipeline {
    agent any

   tools
    {
       maven "maven3"
    }
 stages {
      stage('checkout') {
           steps {

                git branch: 'master', url: 'https://github.com/jaschluc732/CI-CD-using-Docker.git'

          }
        }
      stage('Execute Maven') {
           steps {

                sh 'mvn package'
          }
        }
      stage('Docker Build and Tag') {
           steps {

                sh 'docker build -t samplewebapp:latest .'
                sh 'docker tag samplewebapp jaschlucdocker/samplewebapp:latest'
                //sh 'docker tag samplewebapp jaschlucdocker/samplewebapp:$BUILD_NUMBER'

          }
      }

      stage('Publish image to Docker Hub') {

            steps {
        withDockerRegistry([ credentialsId: "dockerhub", url: "" ]) {
          sh  'docker push jaschlucdocker/samplewebapp:latest'
        //  sh  'docker push jaschlucdocker/samplewebapp:$BUILD_NUMBER'
        }

          }
      }

      stage('Run Docker container on Jenkins Agent') {

            steps
            {
                sh "docker run -d -p 8003:8080 jaschlucdocker/samplewebapp"

            }
      }
      stage('Run Docker container on remote hosts') {

            steps {
            sshagent (credentials: ['jenkins_devhost']){
            sh "docker -H ssh://ubuntu@34.217.32.221 run -d -p 8003:8080 jaschlucdocker/samplewebapp"
            }
          }
      }
    }
 }
