pipeline {
   agent any

   stages {
      stage('Environment review') {
         steps {
            sh 'kubectl version'
            sh 'kubectl get deployments,pods,svc'
         }
      }
      stage('smoke test deploy') {
         steps {
            sh 'kubectl create namespace test'
            sh 'kubectl apply -f deployment.yaml -n test'
         }
      }
   }
}
