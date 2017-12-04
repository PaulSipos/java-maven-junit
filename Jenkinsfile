pipeline {
  agent {
    label 'slave1'
  }

  options {
    buildDiscarder (logRotator(numToKeepStr: '10', artifactNumToKeepStr: '2'))
  }

  stages {
    stage ('Check Maven Install!') {
      steps {
        sh 'mvn -v'
      }
    }

    stage ('Running Unit tests and storing them in Jenkins') {
      steps {
        sh 'mvn test'
        junit 'target/surefire-reports/*.xml'
      }
    }

    stage ('Building the package and archiving it in Jenkins') {
      steps {
        sh 'mvn package'
      }
      post {
        success {
          archiveArtifacts artifacts: 'target/java-maven-junit-helloworld-*.jar', fingerprint: true, onlyIfSuccessful: true
        sh "echo 'Artifact succesfully Archived!'"
        }
      }
    }
  }
}
