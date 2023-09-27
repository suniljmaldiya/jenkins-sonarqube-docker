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
                sh "docker build . -t docker-p"
            }
        }
        stage("Deploy"){
            steps{
                echo "deploy the code"
                sh "docker-compose down && docker-compose up -d"
            }
        }
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
}
