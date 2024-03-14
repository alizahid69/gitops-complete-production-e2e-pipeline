pipeline{
    agent{
        label "jenkins-agent"
    }
    environment{
        APP_NAME = "complete-prodcution-e2e-pipeline"
    }
    stages{
        stage("cleanup Workspace"){
            steps {
                cleanWs()
            }
        }
        stage("Checkout from SCM"){
            steps{
                git branch: 'main', credentialsId: 'alizahid', url: 'https://github.com/alizahid69/gitops-complete-production-e2e-pipeline'
            }
        }
        stage("Update the deployment Tags"){
            steps{
                sh """
                    cat deployment.yaml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                    cat deployment.yaml
                """     
            }
        }
        stage("Push the changed deployment file to git"){
            steps{
                sh """
                    git config --global user.name "alizahid"
                    git config --global user.email "alizahid069@gmail.com"
                    git add deployment.yaml
                    git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'alizahid', gitToolName: 'Default')]){
                  sh "git push https://github.com/alizahid69/gitops-complete-production-e2e-pipeline main"
                }
            }
        }
    }
}
