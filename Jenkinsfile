pipeline {

  environment {
    registry = "chetangautamm/repo"
    registryCredential = '58881f31-29bb-48a8-9da9-fc254654146d' 
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/chetangautamm/jenkinsdemo.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }

    stage('Push Image') {
      steps{
        script {
          docker.withRegistry( "" , registryCredential) {
            dockerImage.push()
          }
        }
      }
    }

    stage('Deploy App') {
      steps {
        sshagent(['k8suser']) {
          sh "scp myweb.yaml k8suser@52.172.221.4:/home/k8suser"
          script {
            try {
              sh "ssh k8suser@52.172.221.4 kubectl apply -f ."
            }catch(error){
              sh "ssh k8suser@52.172.221.4 kubectl apply -f ."
            }
          }
        }              
      }
    }
  }
}
