pipeline {
  environment { 
      registry = "${REGISTRY}"
      registryCredential = "${REGISTRY_CREDENTIALS}"
      dockerImage = '' 
    }
  agent any
  stages {
    stage('Cloning our Git') {
      steps { 
        git branch: 'final-work', url: 'https://github.com/nikita-projects/test-docker.git' 
      }
    } 
    stage('Building our image') { 
      steps { 
        script { 
          dockerImage = docker.build("$REGISTRY", "--no-cache .")
        }
      } 
    }
    stage('Load image to Docker registry') { 
      steps { 
        script { 
          docker.withRegistry( '', registryCredential ) { 
            dockerImage.push("$BUILD_NUMBER") 
          }
        } 
      }
    }
    stage('Start deploy from registry to node') {
      steps {
        build(
          job: 'Prepare node and deploy',
          parameters: [
            //string(name: 'BUILD_NUMBER', value: dockerImage),
            //string(name: 'NODE_IP', value: "${NODE_IP}")
            [
              $class: 'StringParameterValue',
              name: 'BUILD_NUMBER',
              value: "${BUILD_NUMBER}",
            ],
            [
              $class: 'StringParameterValue',
              name: 'NODE_IP',
              value: "${NODE_IP}",
            ],
          ]
        )
      }
    }
    /*stage('Cleaning up') { 
      steps { 
        sh "docker rmi $registry:$BUILD_NUMBER" 
      }
    }*/
  }
  /*post {
    always {
      cleanWs()
    }
  }*/
}
