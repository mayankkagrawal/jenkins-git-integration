pipeline {
  agent any
  environment{
    PATH="/opt/maven/bin:$PATH"
    
  }
  stages {
    stage ("git checkout"){
        steps {
          when (BRANCH_NAME!='master'){
            git credentialsId: 'e8304eb8-7cb5-4b14-99e4-8e5c286c9acc', url: 'https://github.com/mayankkagrawal/jenkins-git-integration'
            } 
          }
        }
    stage ("Maven build"){
        steps {
           sh  "mvn clean package"
        }
      }
    stage('sonar qube'){
      steps {
        sh "/opt/sonar/bin/sonar-scanner -Dsonar.projectKey=mykey -D sonar.projectKey=github-jenkins-sonar -D sonar.sources=./src"
        }
    }
    stage ("store artficat"){
      steps {
         nexusPublisher nexusInstanceId: '1234', nexusRepositoryId: 'releases', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: 'target/jenkins-git-integration.war']], mavenCoordinate: [artifactId: 'jenkins-git-integration', groupId: 'com.amitverma', packaging: 'war', version: '1.0']]]
      }
    }
    stage("deploy"){
      steps {   
          sh label: '', script: 'sudo docker cp target/jenkins-git-integration.war maven:/opt/tomcat/webapps'
        }
      }
     }
  }
