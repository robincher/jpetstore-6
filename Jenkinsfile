pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        echo 'Building Now'
        sh 'mvn clean install -Dlicense.skip=true'
        echo 'Build completed'
      }
    }
    stage('Test') {
      parallel {
        stage('SonarQube Testing') {
          steps {
            sh '''mvn sonar:sonar -Dsonar.host.url=http://54.254.189.208:8081 -Dlicense.skip=true
'''
          }
        }
        stage('Print Tester Credentials') {
          steps {
            echo "The tester is ${TESTER}"
            sleep 10
          }
        }
        stage('Print Build Number') {
          steps {
            echo "This is build number ${BUILD_ID}"
            sleep 20
          }
        }
      }
    }
    stage('JFrog Push') {
      steps {
        echo 'Start JFrog Push'
        script {
          def server = Artifactory.server "artifactory"
          def buildInfo = Artifactory.newBuildInfo()
          def rtMaven = Artifactory.newMavenBuild()

          rtMaven.tool = 'maven'
          rtMaven.deployer server: server, releaseRepo: 'libs-release-local', snapshotRepo: 'libs-snapshot-local'

          buildInfo = rtMaven.run pom: 'pom.xml', goals: "clean install -Dlicense.skip=true"
          buildInfo.env.capture = true
          buildInfo.name = 'jpetstore-6'
          server.publishBuildInfo buildInfo
        }

        echo 'JFrog Push Completed'
      }
    }
    stage('Deploy Prompt') {
      steps {
        input 'Deploy for production?'
      }
    }
    stage('Deployment') {
      steps {
        echo 'Starting deployment'
        echo 'Deployment completed'
      }
    }
  }
  tools {
    maven 'maven'
  }
  environment {
    TESTER = 'robincher'
  }
}