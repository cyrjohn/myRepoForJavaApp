pipeline {
  agent any
 
  tools {
  maven 'Maven3'
  }
  stages {
    stage ('Build') {
      steps {
      sh 'mvn clean install -f MyWebApp/pom.xml'
      }
    }
    stage ('Code Quality') {
      steps {
        withSonarQubeEnv('SonarQube') {
        sh 'mvn -f MyWebApp/pom.xml sonar:sonar'
        }
      }
    }
    stage ('JaCoCo') {
      steps {
      jacoco()
      }
    }
    stage ('Nexus Upload') {
      steps {
      nexusArtifactUploader(
      nexusVersion: 'nexus3',
      protocol: 'http',
      nexusUrl: '192.168.29.207:8081',
      groupId: 'MyWebApp',
      version: '1.0-SNAPSHOT',
      repository: 'maven-snapshots',
      credentialsId: 'Nexus',
      artifacts: [
      [artifactId: 'MyWebApp',
      classifier: '',
      file: 'MyWebApp/target/MyWebApp.war',
      type: 'war']
      ])
      }
    }
    stage ('DEV Deploy') {
      steps {
      echo "deploying to DEV Env "
	  deploy adapters: [tomcat9(credentialsId: 'c288ede0-697f-4065-bc32-5974537e9893', path: '', url: 'http://192.168.29.207:8080/')], contextPath: null, war: '**/*.war'
      }
    }
  }
}
