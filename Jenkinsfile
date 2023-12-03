pipeline {
    agent any
    environment {
        APP_NAME = "complete-prodcution-e2e-pipeline"
      }
    stages{
        stage('Cleanup Workspace') {
            steps {
                cleanWs()
            }
        }
        stage('Checkout From SCM') {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/krishnamsg/DevOps-Pipeline.git'
            }
        }
        stage('Update the Deployemnt Tags') {
            steps {
                sh """
                    cat tomcat-deployment.yaml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' tomcat-deployment.yaml
                    cat tomcat-deployment.yaml
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
                    sh "git push https://github.com/krishnamsg/DevOps-Pipeline.git main"
                    
                }
            }
        }
    }
}
