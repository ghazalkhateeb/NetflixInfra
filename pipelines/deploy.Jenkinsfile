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
                      cd k8s
                      cd $SERVICE_NAME
                      git checkout main
                      # Replace the image name in the deployment YAML file
                      sed -i "s|image:.*|image: ${IMAGE_FULL_NAME_PARAM}|g" deploymentFront.yaml


                      # Print the updated deployment YAML to verify the change
                      echo "Updated deploymentFront.yaml:"
                      cat deploymentFront.yaml

                      git config --global user.email "jenkins@example.com"
                      git config --global user.name "Jenkins"

                      git remote set-url origin https://${GITHUB_TOKEN_PSW}@github.com/ghazalkhateeb/NetflixInfra.git


                      git add deploymentFront.yaml
                      git commit -m "Update deployment image to ${IMAGE_FULL_NAME_PARAM}"
                      git push origin main

                    '''


                }
            }
        }
    }
}