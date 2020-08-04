pipeline {
  environment {
    registry = "gbarska/test-pipeline"
    registryCredential = 'gbarska-hub'
    dockerImage = ''
  }
 agent {
        dockerfile {
            filename 'Dockerfile'
            reuseNode true
        }

     stage('SCM'){ 
          steps{
                sh 'echo "Cloning..."'
         }     
    }

    stage('Test'){ 
          steps{
                sh 'echo "Testing..."'
         }     
    }

    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }

    stage('Deploy Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }

    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $registry:$BUILD_NUMBER"
      }
    }
 }
 
 
}
