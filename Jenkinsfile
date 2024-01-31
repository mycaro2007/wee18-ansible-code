pipeline {
    agent any

    stages {
        stage('Zip Ansible Code') {
            steps {
                // Zip the contents of the Ansible directory, excluding the Jenkinsfile
                sh 'zip -r ansible-codes.zip wee18-ansible-code-x Jenkinsfile'
            }
        }

        stage('Upload to JFrog Artifactory') {
            steps {
                // Pushing the code to Jfrog Artifactory
                sh 'curl -uadmin:AP3oJAo4EgQpQnCMyCyepW1iVFf -T ansible-codes.zip "http://54.89.215.83:8081/artifactory/Jenkins-Ansible-code/ansible-codes.zip"'
            }
        }

        stage('Deploy to Ansible Server') {
            agent {
                label 'ansible'
            }
            steps {
                // Download the zipped Ansible code from JFrog Artifactory
                sh 'curl -uadmin:AP3oJAo4EgQpQnCMyCyepW1iVFf -O "http://54.89.215.83:8081/artifactory/Jenkins-Ansible-code/ansible-codes.zip"'

                // Add the Ansible server to the known hosts to avoid host verification issues
                sh 'ssh-keyscan -H 3.82.41.54 >> ~/.ssh/known_hosts'

                // Transfer the zip file to the Ansible server
                sh 'scp ansible-codes.zip ec2-user@3.82.41.54:/home/ec2-user/'

                // Unzip the file on the Ansible server
                sh 'ssh ec2-user@3.82.41.54 "unzip -o /home/ec2-user/ansible-codes.zip -d /home/ec2-user/ansible-dev/"'
            }
        }

        stage('Run playbook') {
            agent {
                label 'ansible1'
            }
            steps {
                script {
                    // Unzip ansible-codes.zip
                    sh 'unzip -o ansible-codes.zip'
                    // Run ansible-playbook from the correct directory
                    dir('ansible-codes') {
                        sh 'ansible-playbook -i /home/ec2-user/ansible-dev/week18-ansible-code/inventory.yml /home/ec2-user/ansible-dev/week18-ansible-code/play1.yml'
                    }
                }
            }
        }
    }
}
