pipeline {
    agent any

    stages {
        stage('Zip Ansible Code') {
            steps {
                script {
                    // Change to the directory where your Ansible code is located
                    dir('Week18-Ansible-Code') {
                        // Zip the contents of the Ansible directory, excluding the Jenkinsfile
                        sh 'zip -r ansible-codes.zip * -x Jenkinsfile'
                    }
                }
            }
        }

        stage('Upload to JFrog Artifactory') {
            steps {
                // Ensure the zip file path is correct and accessible from the root of the workspace
                // Pushing the code to Jfrog Artifactory
                sh 'curl -uadmin:AP3oJAo4EgQpQnCMyCyepW1iVFf -T Week18-Ansible-Code/ansible-codes.zip "http://54.89.215.83:8081/artifactory/Jenkins-Ansible-code/ansible-codes.zip"'
            }
        }

        // Other stages like build, test, deploy, etc. can be added here
    }

    post {
        // Post actions like cleanup, notifications, etc. can be added here
    }
}

