pipeline {
    agent any
    parameters {
        string(defaultValue: "", description: '', name: 'BUILDNUMBER')
    }
    environment{
        VERSION = "${BUILDNUMBER}"
    }
   
    stages{
        stage('Update latest tag in kubernetes deployment file in Jenkins local workspace'){
            steps{
                script{                    
                    sh """
                        cat deploymentservice.yaml
                        sed -i 's+fitoni/gitops.*+fitoni/gitops:${VERSION}+g' deploymentservice.yaml
                        cat deploymentservice.yaml
                    """                       
                }
            }
        }

        stage('Push that newly updated kubernetes deployment file onto GitHub - the CD Part'){
            steps{
                script{
                    withCredentials([usernamePassword(credentialsId: 'jenkinsPushToGithub', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                    sh """
                        git config user.email fitoni77@gmail.com
                        git config user.name fitoni
                        git add deploymentservice.yaml
                        git commit -m "This is done by Jenkins Job changemanifest: ${VERSION}"
                        git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/bs4-admin-cd.git HEAD:main
                    """               
                    }
                }
            }
        } 
    }
}