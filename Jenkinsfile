pipeline {
    agent { label "Jenkins-Agent" }
    environment {
        APP_NAME = "register-app-pipeline"
        IMAGE_TAG = "v1.0.0"
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/yashdavkhar/gitops-register-app'
            }
        }

        stage("Update the Deployment Tags") {
            steps {
                sh """
                   sed -i "s|image:.*|image: ${APP_NAME}:${IMAGE_TAG}|g" deployment.yaml
                   echo "Updated deployment.yaml:"
                   cat deployment.yaml
                """
            }
        }

        stage("Push the changed deployment file to Git") {
            steps {
                withCredentials([usernamePassword(credentialsId: 'github', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                    sh """
                       git config --global user.name "yashdavkhar"
                       git config --global user.email "ashfaque.s510@gmail.com"
                       git add deployment.yaml
                       git commit -m "Updated Deployment Manifest" || echo "No changes to commit"
                       git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/yashdavkhar/gitops-register-app
                    """
                }
            }
        }
    }
}
