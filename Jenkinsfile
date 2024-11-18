node {
    def app

    stage('Clone repository') {
        checkout scm
    }

    stage('Update GIT') {
        script {
            // Catch errors and mark stage as failed but continue pipeline execution
            catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                // Use SSH credentials stored in Jenkins
                sshagent(['gitlogin']) { // Replace with your Jenkins credential ID
                    sh "git config user.email busc9999@gmail.com"
                    sh "git config user.name budaysagar"
                    
                    // Display current deployment.yaml for debugging
                    sh "cat deployment.yaml"

                    // Update deployment.yaml with the new Docker tag
                    sh "sed -i 's+budaysagar/test.*budaysagar/test:${DOCKERTAG}+g' deployment.yaml"
                    
                    // Show updated file for verification
                    sh "cat deployment.yaml"

                    // Add and commit the changes
                    sh "git add deployment.yaml"
                    sh "git commit -m 'Updated manifest by Jenkins Job: ${env.BUILD_NUMBER}' || echo 'No changes to commit'"
                    
                    // Push changes to the main branch using SSH URL
                    sh "git push git@github.com:budaysagar/kubernetesmanifest.git HEAD:main"
                }
            }
        }
    }
}
