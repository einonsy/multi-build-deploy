pipeline {
   agent any
   stages {
      stage('Environment review') {
         steps {
            sh 'kubectl version'
            sh 'kubectl get deployments,pods,svc'
         }
      }

   // TODO Unit Test


      stage('Build Images') {
         steps {
            sh "docker build -t einonsy/apache2:${env.BUILD_NUMBER} ."
         }
      }

      stage('push images') {
         steps {
            withCredentials([usernamePassword(credentialsId: 'docker-hub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
            sh "docker login -u ${env.dockerHubUSER} -p ${env.dockerHubPassword}"
            sh "docker push einonsy/apache2:${env.BUILD_NUMBER}"
         }
            }
      }
      
   }
}
