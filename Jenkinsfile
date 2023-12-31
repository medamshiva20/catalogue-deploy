pipeline {
    agent { node { label 'AGENT-1' } }
    options{
        ansiColor('xterm')
    }
     parameters {
        string(name: 'version', defaultValue: '1.0.1', description: 'Which version to deploy')
    }
    stages{
        stage('Deploy')
        {
        steps{
                echo "Deploying..."
                echo "Version from params: ${params.version}"
            }
         }
          stage('Init')
        {
        steps{
                sh '''

                cd terraform
                terraform init -reconfigure

                '''
            }
         }
         stage('Plan')
        {
        steps{
                sh '''

                cd terraform
                terraform plan -var="app_version=${params.version}"
                
                '''
            }
         }
         stage('Approve') {
            input {
                message "Should we continue?"
                ok "Yes, we should."
                submitter "alice,bob"
                parameters {
                    string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
                }
            }
            steps {
                echo "Hello, ${PERSON}, nice to meet you."
            }
        }

        stage('Apply'){
            steps{
                sh """
                cd terraform
                terraform apply -var="app_version=${params.version}" -auto-approve
                """
            }
        }
    }
        post{
            always{
                echo 'cleaning up workspace'
                deleteDir()
            }
        }
}