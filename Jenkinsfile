pipeline {
  agent {
    docker {
      image 'bitwiseman/training-blueocean-sample'
      args '-u root -v $HOME/.m2:/root/.m2'
    }
    
  }
  stages {
    stage('Build') {
      steps {
        sh './jenkins/build.sh'
        archiveArtifacts 'target/*.war'
      }
    }
    stage('Archive') {
      steps {
        archiveArtifacts 'target/*.war'
      }
    }
    stage('Test') {
      steps {
        sh './jenkins/test-all.sh'
        junit '**/test-results/karma/*.xml'
      }
    }
  }
}