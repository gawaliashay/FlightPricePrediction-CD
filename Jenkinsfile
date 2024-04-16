pipeline {
    agent any
    environment {
                APP_NAME = "flight"
                RELEASE = "1.0.0"
                IMAGE_TAG = 
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
               steps {
                   git branch: 'main', credentialsId: 'GitHub', url: 'https://github.com/gawaliashay/FlightPricePrediction-CD'
               }
        }

        stage("Update the Deployment Tags") {
            steps {
                sh """
                   cat deployment.yaml
                   sed -i "s/${RELEASE}.*/${IMAGE_TAG}/g" deployment.yaml
                   cat deployment.yaml
                """
            }
        }

        stage("Push the changed deployment file to Git") {
            steps {
                sh """
                   git config --global user.name "gawaliashay"
                   git config --global user.email "gawali.ashay@gmail.com"
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'GitHub', gitToolName: 'Default')]) {
                  sh "git push https://github.com/gawaliashay/FlightPricePrediction-CD main"
                }
            }
        }
      
    }
}
