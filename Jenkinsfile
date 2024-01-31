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
                sh 'curl -uadmin:AP3oJAo4EgQpQnCMyCyepW1iVFf -T ansible-codes.zip "http://3.83.42.163:8081/artifactory/Jenkins-Ansible-code/ansible-codes.zip"'
            }
        }

        stage('Deploy to Ansible Server') {
            agent {
                label 'ansible'
            }
            steps {
                // Download the zipped Ansible code from JFrog Artifactory
                sh 'curl -uadmin:AP3oJAo4EgQpQnCMyCyepW1iVFf -O "http://3.83.42.163:8081/artifactory/Jenkins-Ansible-code/ansible-codes.zip"'

            }
        }
        stage('Run playbook') {
            agent {
                label 'ansible'
            }
            steps {
                script {
                    // Unzip ansible-codes.zip
                    sh 'unzip -o ansible-codes.zip'
                    // Run ansible-playbook from the correct directory
                    dir('ansible-codes') {
                        sh 'ansible-playbook -i /home/ec2-user/ansible-dev/inventory.yml /home/ec2-user/ansible-dev/code2.yml'
                    }
                }
            }
        }
    }
}
