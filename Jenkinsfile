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
                sh 'sleep 5'
            }
        }

        stage('Terraform Format') {
            steps {
                sh 'java -version'
            }
        }

        stage('Terraform Init') {
            steps {
                sh 'python -version'
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
