pipeline {
    agent { label 'vinod' }
    environment {
              APP_NAME = "register-app-pipeline"
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
               steps {
                    git branch: 'main', credentialsId: 'github', url: 'https://github.com/rishihudar/gitops-register-app'
               }
        }

        stage("Update the Deployment Tags") {
            steps {
                sh """
                   cat deployment.yaml
                   sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                   cat deployment.yaml
                """
            }
        }

        stage("Push the changed deployment file to Git") {
            steps {
                sh """
                   git config --global user.name "rishihudar"
                   git config --global user.email "rishiaws100@gmail.com"
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'Github-cred', gitToolName: 'Default')]) {
                  sh "git push https://github.com/rishihudar/gitops-register-app main"
                }
            }
        }
      
    }
}
