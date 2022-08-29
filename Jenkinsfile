pipeline {
  agent {
      node { 
          label  'master'
          
      }
  }
  environment {
     PATH = "/usr/local/bin:${env.PATH}"
     nodeimagename = "jenkins/multi-node-repo:"
     registryCredential = 'suriya_docker'
      dockerImage = ''
  }
  stages {
    stage('Build'){
      steps{
         
        echo "build success"
        }
      }    
    stage ('Deploy'){
      steps{
        script {
          dockerImage = docker.build("${nodeimagename}build-${BUILD_NUMBER}")
          docker.withRegistry('', registryCredential) {
            dockerImage.push()
          }
        }
      }
    }
    // stage('cleanup'){
    //   steps{
    //      //sh 'sudo docker rmi -f $(docker images -aq)'
    //      echo "Clean Up Running."
    //   }
    // }
    stage('DEV-QA-stage'){
      steps{
        build job: 'sample job1', parameters: [string(name: 'VALUE_BUILD', value:"$BUILD_NUMBER")], propagate: false
      }
    }
  }
}