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
                sh 'ls -la'
            }
        }

        stage('Terraform Version') {
            steps {
                sh 'sleep 10'
            }
        }

        stage('Terraform Format') {
            steps {
                sh 'pwd'
            }
        }

        stage('Terraform Init') {
            steps {
                sh 'sleep 5'
            }
        }

        stage('Terraform Plan') {
            steps {
                sh 'date'
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
