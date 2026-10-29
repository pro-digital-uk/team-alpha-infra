pipeline {
    agent any

    options {
        disableConcurrentBuilds()
    }

    stages {
        stage('Git Clone') {
            steps {
                sh '''
                    git clone https://github.com/pro-digital-uk/team-alpha-infra.git
                    git checkout develop
                '''
            }
        }

        stage('Terraform List ') {
            steps {
                sh 'cd team-alpha-infra'
            }
        }

        stage('Terraform Version') {
            steps {
                sh 'terraform version'
            }
        }

        stage('Terraform Format') {
            steps {
                sh 'terraform fmt'
            }
        }

        stage('Terraform Init') {
            steps {
                sh 'terraform init'
            }
        }

        stage('Terraform Plan') {
            steps {
                sh 'terraform plan'
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
