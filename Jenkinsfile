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
            script {
               for dockerfile in docker/Dockerfile.*; do
                  project=${dockerfile##*\.}
                  docker build -t docker.${project}:${BRANCH_NAME}-latest -f ${dockerfile} .

    returnStatus=$?
    if [ $returnStatus -ne 0 ]; then
      exit $returnStatus
    fi
done
unset project
unset dockerfile
            }
            sh "docker build -t einonsy/podinfo:${env.BUILD_NUMBER} --file app01/."
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
