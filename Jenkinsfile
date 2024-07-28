pipeline {
    agent any
    environment {
          APP_NAME = "reddit-clone-pipeline"
    }
    stages {
         stage("Cleanup Workspace") {
             steps {
                cleanWs()
             }
         }
         stage("Checkout from SCM") {
             steps {
                     git branch: 'main', credentialsId: 'github-ssh-key', url: 'https://github.com/shekharSamal9/reddit-clone-gitops.git'
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
         stage("Push the changed deployment file to GitHub") {
            steps {
                sh """
                    git config --global user.name "shekharSamal9"
                    git config --global user.email "thisisshekhar9@gmail.com"
                    git add deployment.yaml
                    git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                    // sh "git push git@github.com:shekharSamal9/reddit-clone-gitops.git main"
                     sshagent(['github-ssh-key']) {
                           sh 'git push https://github.com/shekharSamal9/reddit-clone-gitops.git main'
                      }
                }
            }
         }
    }
}
