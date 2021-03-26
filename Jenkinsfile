pipeline {
  environment {
    imagename = "rishabh911996/simplilearn-devops-cert"
    registryCredential = 'docker-hub'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        git([url: 'https://github.com/rishabh911996/simplilearn-devops-cert-2.git', branch: 'master', credentialsId: 'git-hub-1'])

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
    stage('Deploy image and Remove Unused  image') {
      steps{
        sh "docker stop simplilearn-devops-cert"
        sh "docker rm simplilearn-devops-cert"
        sh "docker run -d -p 80:5050 --name simplilearn-devops-cert $imagename:$BUILD_NUMBER"
        sh "docker rmi $imagename:$BUILD_NUMBER"

      }
    }
  }
}
