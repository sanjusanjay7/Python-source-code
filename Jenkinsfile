pipeline {
    agent any

    environment{
        DOCKERHUB_USERNAME = "sanjaykumar70"
        APP_NAME = "packages"
        IMAGE_NAME = "${DOCKERHUB_USERNAME}" + "/" + "$(APP_NAME}"
        REGISTRY_CREDS = 'dockerhub'
    }
    
    stage('Clone repository') {
      

        checkout scm
        steps{
            script{
                git credentialsId: 'github',
                url: 'https://github.com/sanjusanjay7/Python-source-code.git'
            }
            }
        }   

    stage('Build image') {
  
       app = docker.build("sanjaykumar70/packages")
    }

    stage('Test image') {
  

        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Push image') {
        
        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            app.push("${env.BUILD_NUMBER}")
        }
    }
    
    stage('Trigger ManifestUpdate') {
                echo "triggering updatemanifestjob"
                build job: 'updatemanifest', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
        }
}
