node {
    def app

    stage('Clone repository') {
        checkout scm
    }

    stage('Update GIT') {
        script {
            catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                    sh "git config user.email 'shankar.c101189@gmail.com'"
                    sh "git config user.name 'Gowrishankarc'"

                    // Update the image tag in the deployment.yaml file
                    echo 'Updating deployment.yaml with new image tag...'
                    sh "sed -i 's+8197495215/i18next-app:.*+8197495215/i18next-app:${params.DOCKERTAG}+g' deployment.yaml"

                    // Confirm the changes
                    sh "cat deployment.yaml"

                    // Commit the changes
                    sh "git add deployment.yaml"
                    sh "git commit -m 'Updated deployment file with new image tag: ${params.DOCKERTAG}'"
                    sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/kubernetesmanifest.git HEAD:main"
                }
            }
        }
    }
}