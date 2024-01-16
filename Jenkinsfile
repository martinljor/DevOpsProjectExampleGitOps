pipeline {
    agent { label "jenk-agent" }
    environment {
              APP_NAME = "devopsprojectexample-testfile"
    }

    stages {
        stage("Cleanup") {
            steps {
                cleanWs()
            }
        }

        stage("Check") {
               steps {
                   git branch: 'main', credentialsId: 'GitHub-martinljor', url: 'https://github.com/martinljor/DevOpsProjectExampleGitOps'
               }
        }

        stage("Update deployment tags") {
            steps {
                sh """
                   cat deployment.yaml
                   sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                   cat deployment.yaml
                """
            }
        }

        stage("Push to Git") {
            steps {
                sh """
                   git config --global user.name "martinljor"
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'GitHub-martinljor', gitToolName: 'Default')]) {
                  sh "git push https://github.com/martinljor/DevOpsProjectExampleGitOps main"
                }
            }
        }
      
    }
}
