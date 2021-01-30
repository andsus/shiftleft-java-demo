pipeline {
  agent {
    node {
      label 'docker'
    }

  }
  stages {
    stage('Build & Test') {
      agent {
        docker {
          image 'nikhilpathania/jenkins_ssh_agent'
        }

      }
      steps {
        sh 'mvn -Dmaven.test.failure.ignore clean package'
        stash(includes: '**/target/surefire-reports/TEST-*.xml,target/*.jar', name: 'build-test-artifacts')
      }
    }

    stage('Report & Publish') {
      agent {
        node {
          label 'docker'
        }

      }
      steps {
        unstash 'build-test-artifacts'
        junit '**/target/surefire-reports/TEST-*.xml'
        archiveArtifacts(artifacts: 'target/*.jar', onlyIfSuccessful: true)
      }
    }

  }
}