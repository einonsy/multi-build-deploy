pipeline {
   agent any
   stages {
      stage('Environment review') {
         steps {
            sh 'kubectl version'
            sh 'kubectl get deployments,pods,svc'
            sh 'ls -als'
         }
      }

   // TODO Unit Test


      stage('Build Images') {
         steps {
            sh "docker build -t einonsy/podinfo:${env.BUILD_NUMBER} --file app01/dockerfile"
         }
      }

      stage('push images') {
         steps {
            withCredentials([usernamePassword(credentialsId: 'docker-hub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
            sh "docker login -u ${env.dockerHubUSER} -p ${env.dockerHubPassword}"
            sh "docker push einonsy/podinfo:${env.BUILD_NUMBER}"
         }
            }
      }
      stage('Docker Remove Image') {
         steps {
            sh "docker rmi einonsy/podinfo:${env.BUILD_NUMBER}"
         }
      }
      
      stage('Apply Kubernetes Files') {
         steps {
            
            sh 'cat deployment.yaml | sed "s/{{BUILD_NUMBER}}/$BUILD_NUMBER/g" | kubectl apply -f -'
            sh 'kubectl apply -f service.yaml'
            
         }
      }

   }
}
