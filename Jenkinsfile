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
        junit allowEmptyResults: true, healthScaleFactor: 0.1, testResults: 'target/surefire-reports/*.xml'
      }
    }

    stage ('Running Integration tests and storing the results in Jenkins'){
      steps {
        sh 'mvn verify'
        junit allowEmptyResults: true, healthScaleFactor: 0.1, testResults: 'target/failsafe-reports/*.xml'
      }
    }

    stage ('Code quality test with SonarQube') {
      steps {
        withSonarQubeEnv('Sonar.Local') {
          sh 'mvn sonar:sonar'
        }
      }

    }
    stage ('Building the package and archiving it in Jenkins') {
      steps {
        sh 'mvn clean package'
      }
      post {
        success {
        archiveArtifacts artifacts: 'target/java-maven-junit-helloworld-*.jar', fingerprint: true, onlyIfSuccessful: true
        sh "echo 'Artifact succesfully Archived!'"
        sh "echo 'Publishing Coverage Report!'"
        cobertura autoUpdateHealth: false, autoUpdateStability: false, coberturaReportFile: 'target/site/cobertura/coverage.xml', conditionalCoverageTargets: '70, 60, 60', failNoReports: false, failUnhealthy: false, failUnstable: false, lineCoverageTargets: '80, 70, 70', maxNumberOfBuilds: 10, methodCoverageTargets: '90, 85, 85', onlyStable: false, sourceEncoding: 'ASCII', zoomCoverageChart: false
        }
      }
    }



  }
}
