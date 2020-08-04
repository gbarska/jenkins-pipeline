pipeline {
  environment {
    registry = "gbarska/test-pipeline"
    registryCredential = 'gbarska-hub'
    dockerImage = ''
  }
  agent any {


  stages {
    stage('Cloning Git') {
      steps {
       	checkout([$class: 'GitSCM', branches: [[name: '*/Develop']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/gbarska/jenkins-net']]])
      }
    }
    stage('Build') {
          steps {
               sh 'dotnet build "$WORKSPACE/src/EAApp/EAApp.csproj"'
          }
    }
 }
}
 agent {
        dockerfile {
            filename 'Dockerfile'
            reuseNode true
        }
    stage('Test'){ 
          steps{
                    sh 'echo "hello"'
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
