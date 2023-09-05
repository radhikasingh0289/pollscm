pipeline {

  environment {
    imagename = "radhikasinghkirar/playjenkins"
    registryCredential = 'dockerhub'
    dockerImage = ''
    registry = "radhikasinghkirar/playjenkins"
    KUBECONFIG = credentials('kubeconfigmaster')
  }

  agent any

  stages {

    stage('Cloning Git') {
      steps {
        git([url: 'https://github.com/radhikasingh0289/playjenkins.git', branch: 'master', credentialsId: 'githubaccesstoken'])
 
      }
    }

   stage('Building image') {
      steps{
        script {
          dockerImage = docker.build imagename
        }
      }
    }

    stage('Push Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push("$BUILD_NUMBER")
             dockerImage.push('latest')
          }
        }
      }
    }

    stage('Deploy App') {
      steps {
        script {
          sh "kubectl --kubeconfig=$KUBECONFIG apply -f myweb.yaml"
        }
      }
    }

  }

}
