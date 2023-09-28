node {
  stage('SCM') {
    checkout scm
  }
  stage('SonarQube Analysis') {
    def scannerHome = tool 'SonarScanner';
    withSonarQubeEnv() {
      sh "${scannerHome}/bin/sonar-scanner"
    }
  }
}
pipeline {
    agent any
    stages{
        stage("code"){
            steps{
                echo "pulling the code from github"
                git url:"https://github.com/suniljmaldiya/jenkins-sonarqube-docker.git",branch:"main"
            }
        }
        stage("Build"){
            steps{
                echo "Building the code"
                sh "docker build . -t newapp"
            }
        }
        stage("Deploy"){
            steps{
                echo "deploy the code"
                sh "docker-compose down && docker-compose up -d"
            }
        }
      stage("Push to Dockerhub"){
        steps{
          echo "push image to docker hub"
          withCredentials([usernamePassword(credentialsId:"dockerhubid",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
          sh "docker tag newapp ${env.dockerHubUser}/myapp:latest"
          sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
          sh "docker push ${env.dockerHubUser}/myapp:latest"
          }
    }
}
    }

