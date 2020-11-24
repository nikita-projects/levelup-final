pipeline {
  environment { 
      registry = $REGISTRY 
      registryCredential = $REGISTRY_CREDENTIALS 
      dockerImage = '' 
    }
  agent { dockerfile true }
  stages {
    stage('Cloning our Git') {
      steps { 
        git branch: 'final-work', url: 'https://github.com/nikita-projects/test-docker.git' 
      }
    } 
    stage('Building our image') { 
      steps { 
        script { 
          dockerImage = docker.build registry + ":$BUILD_NUMBER" 
        }
      } 
    }
    stage('Load image to Docker registry') { 
      steps { 
        script { 
          docker.withRegistry( '', registryCredential ) { 
          dockerImage.push() 
        }
      } 
    }
    stage('Start deploy from registry to node') {
      build job: 'Prepare node and deploy', parameters: [
          string(name: 'DOCKER_IMAGE', value: dockerImage),
          string(name: 'NODE_IP', value: NODE_IP)
        ]
    }
    stage('Cleaning up') { 
      steps { 
        sh "docker rmi $registry:$BUILD_NUMBER" 
      }
    } 
  }
  post {
    always {
      cleanWs()
    }
  }
}