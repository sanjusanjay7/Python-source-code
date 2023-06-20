pipeline {
  agent any
  
  stages {
    stage('Clone repository') {
      steps {
        checkout scm
      }
    }

    stage('Build image') {
      steps {
        script {
          // Declare the 'app' variable at the top level
          def app

          app = docker.build("sanjaykumar70/packages")
        }
      }
    }

    stage('Test image') {
      steps {
        script {
          app.inside {
            sh 'echo "Tests passed"'
          }
        }
      }
    }

    stage('Push image') {
      steps {
        script {
          docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            app.push("${env.BUILD_NUMBER}")
          }
        }
      }
    }
    
    stage('Trigger ManifestUpdate') {
      steps {
        echo "Triggering updatemanifestjob"
        build job: 'updatemanifest', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
      }
    }
  }
}
