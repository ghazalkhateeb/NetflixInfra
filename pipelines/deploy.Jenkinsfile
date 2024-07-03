pipeline {
    agent {
       label 'general'
    }

    parameters {
        string(name: 'SERVICE_NAME', defaultValue: '', description: '')
        string(name: 'IMAGE_FULL_NAME_PARAM', defaultValue: '', description: '')
    }
    environment {
        GITHUB_TOKEN = credentials('fe1ad36c-199b-49db-893a-38b60eb82288')
    }

    stages {
        stage('Deploy') {
            steps {
                script {
                    // Update the YAML manifests with the new image name
                    sh '''

                      cd $SERVICE_NAME
                      # Replace the image name in the deployment YAML file
                      sed -i "s|image:.*|image: ${IMAGE_FULL_NAME_PARAM}|g" k8s/deploymentFront.yaml

                      # Print the updated deployment YAML to verify the change
                      echo "Updated deploymentFront.yaml:"
                      cat k8s/NetflixFronted/deploymentFront.yaml
                    '''

                    // Commit the changes
                    sh '''
                      git config --global user.email "jenkins@example.com"
                      git config --global user.name "Jenkins"
                      TOKEN = "${GITHUB_TOKEN_PSW}"
                      git remote set-url origin https://${TOKEN}@github.com/ghazalkhateeb/NetflixInfra.git


                      git add k8s/NetflixFronted/deploymentFront.yaml
                      git commit -m "Update deployment image to ${IMAGE_FULL_NAME_PARAM}"
                      git push origin HEAD:main
                    '''
                }
            }
        }
    }
}