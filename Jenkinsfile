pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        echo 'Building Now'
        sh 'mvn clean install -Dlicense.skip=true'
      }
    }
  }
}