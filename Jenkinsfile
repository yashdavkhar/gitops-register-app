pipeline {
    agent { label "Jenkins-Agent" }
    environment {
        APP_NAME = "register-app-pipeline"
        IMAGE_TAG = "v1.0.0" // Replace with your dynamic tag logic
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
                   echo "APP_NAME: ${APP_NAME}"
                   echo "IMAGE_TAG: ${IMAGE_TAG}"
                   echo "Before update:"
                   cat deployment.yaml
                   sed -i "s|image:.*|image: ${APP_NAME}:${IMAGE_TAG}|g" deployment.yaml
                   echo "After update:"
                   cat deployment.yaml
                """
            }
        }

        stage("Push the changed deployment file to Git") {
            steps {
                sh """
                   git config --global user.name "yashdavkhar"
                   git config --global user.email "ashfaque.s510@gmail.com"
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest" || echo "No changes to commit"
                   git push https://github.com/yashdavkhar/gitops-register-app
                """
            }
        }
    }
}
