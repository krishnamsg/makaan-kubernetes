pipeline {
    agent any
    environment {
        APP_NAME = "makaam-application"
      }
    stages{
        stage('Cleanup Workspace') {
            steps {
                cleanWs()
            }
        }
        stage('Checkout From SCM') {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/krishnamsg/makaan-kubernetes.git'
            }
        }
        stage('Update the Deployemnt Tags') {
            steps {
                sh """
                    cat makaan-deployment.yaml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' makaan-deployment.yaml
                    cat makaan-deployment.yaml
                """
            }
        }
        stage('Push the changed deployment to GIT') {
            steps {
                sh """
                    git config --global user.name "krishnamsg"
                    git config --global user.email "krishnams.aws1@gmail.com"
                    git add .
                    git commit -m "Updated deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                    sh "git push https://github.com/krishnamsg/makaan-kubernetes.git main"
                    
                }
            }
        }
    }
}
