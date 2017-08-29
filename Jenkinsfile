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
    stage('Backend') {
      steps {
        parallel(
          "Backend": {
            sh './jenkins/test-backend.sh'
            junit '**/surefire-reports/**/*.xml'
            
          },
          "Frontend": {
            sh './jenkins/test-frontend.sh'
            archiveArtifacts '**/test-results/karma/*.xml'
            
          },
          "Static": {
            sh './jenkins/test-static.sh'
            
          },
          "Performance": {
            sh './jenkins/test-performance.sh'
            
          }
        )
      }
    }
  }
}