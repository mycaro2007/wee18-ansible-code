pipeline {
    agent any

    stages {
        stage('Zip Ansible Code') {
            steps {
                script {
                    // Change to the directory where your Ansible code is located
                    dir('Week18-Ansible-Code') {
                        // Zip the contents of the ansible directory
                        sh 'zip -r ansible-codes.zip * -x Jenkinsfile'
                    }
                }
            }
        stage{
            steps{
                //Pushing the code to Jfrog artifactory
               sh 'curl -uadmin:AP3oJAo4EgQpQnCMyCyepW1iVFf -T <PATH_TO_FILE> "http://54.89.215.83:8081/artifactory/Jenkins-Ansible-code/<TARGET_FILE_PATH>"' 
            }

        }
        }

        // Other stages like build, test, deploy, etc.
    }

    // Post actions if any
}
