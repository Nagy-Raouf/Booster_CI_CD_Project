pipeline {
    agent {label 'slave'}

    stages {
        stage('Build') {
                steps {
                    sh 'docker build -f dockerfile . -t nagyraouf/dajngoo:v1.0'
                      }
                   post{
                    success{
                         slackSend(color: '#00FF00',message: "BUILDSTART: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'(${env.BUILD_URL}console)")
                      }
                  }
            }
        stage('Push') {
            steps {
                withCredentials([usernamePassword(credentialsId:"docker",usernameVariable:"USERNAME",passwordVariable:"PASSWORD")]){
                sh 'docker login --username $USERNAME --password $PASSWORD'
                sh 'docker push nagyraouf/dajngoo:v1.0'
                }
            }
        }
         stage('Deploy') {
            steps {
                sh 'docker run -d -p 5000:3000 nagyraouf/dajngoo:v1.0'
            }  
    }
  }
}
