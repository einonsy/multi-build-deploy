pipeline {
   agent any

   stages {
      stage('Environment review') {
         steps {
            sh 'kubectl version'
            sh 'kubectl get deployments,pods,svc'
         }
      }
   }
}
