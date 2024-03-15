pipeline {
    agent 
       agent any
    environment {
               APP_NAME = "test"
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
//                cleanWs()
                  deleteDir()
            }
        }

        stage("Checkout from SCM") {
               steps {
                   git branch: 'test-cd', credentialsId: 'git-hub', url: 'https://github.com/DEL-ORG/s7auriole-revive.git'
                    }
               }
        }

        stage("Update the Deployment Tags") {
            steps {
                sh """
                   cat deployment.yaml
                   sed -i 's/${APP_NAME}.*/${APP_NAME}_${IMAGE_TAG}/g' deployment.yaml
                   cat deployment.yaml
                """
            }
        }

        stage("Push the changed deployment file to Git") {
            steps {
                sh """
                   git config --global user.name "s7testar"
                   git config --global user.email "attamegnon@gmail.com"
                   git pull --all
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'git-hub', gitToolName: 'Default')]) {
                  sh "git push https://github.com/DEL-ORG/s7auriole-revive.git test-cd"
                }
            }
        }
      
    }

