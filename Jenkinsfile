pipeline {
agent any
tools{
maven 'Maven3.6.3'
}
environment {
DOCKER_TAG = getVersion()
registryCredential="dockerhub"
registry="houaida0501/stockmanagement:${DOCKER_TAG}"
}
stages {
stage ('Initialize') {
steps {
git branch: 'main', url: 'https://github.com/Houaida05/Devops_Project'
}
}
stage ('Maven Build') {
steps {
sh 'mvn clean package'
}
}
stage('SonarTests') {
      steps {
   sh 'sudo docker run -v $(pwd):/usr/src -e SONAR_HOST_URL="http://172.16.110.151:9000" sonarsource/sonar-scanner-cli'
      } 
    }
stage ('Docker Build') {
steps {
sh 'sudo docker build . -t houaida0501/stockmanagement:${DOCKER_TAG}'
}
}
	// Upload image to docker hub
     stage('Upload Image') {
     steps{    
         script {
            docker.withRegistry( '', registryCredential ) {
           sh 'sudo docker push houaida0501/stockmanagement:${DOCKER_TAG}'
            }
        }
      }
    }
}
}
def getVersion(){
def version = sh returnStdout: true, script: 'git rev-parse --short HEAD'
return version
}
