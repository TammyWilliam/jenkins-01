pipeline {
  agent any
  tools {
    maven 'maven'
  }
  stages{
    stage('1-cloning project repo'){
      steps{
        checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'github', url: 'https://github.com/TammyWilliam/jenkins-01.git']])
      }
    }
    stage('2-cleanws'){
      steps{
        sh 'mvn clean'
      }
    }
    stage('3-mavenbuild'){
      steps{
        sh 'mvn package'
      }
    }
    stage('codequality'){
        steps{
       sh "mvn clean verify sonar:sonar \
  -Dsonar.projectKey=team9 \
  -Dsonar.host.url=http://3.15.0.191:9000 \
  -Dsonar.login=sqp_15131ba60b4a38192f8812c095af68c0c624e1e2"
      }
    }
    stage('5-deploy-to-tomcat'){
      steps{
        sshagent(['tomcat']){
          sh """
          scp -o StrictHostKeyChecking=no ~/workspace/maven-build/MavenEnterpriseApp-web/target/MavenEnterpriseApp.war ubuntu@3.144.193.205:/opt/tomcat/apache-tomcat-9.0.88/webapps
          """
        }
      }
    }
  }
}