pipeline {
    agent any

    parameters {
        string(name: 'BRANCH', defaultValue: 'develop', description: 'Git branch to checkout (e.g., main, develop, feature-branch)')
    }

    options {
        disableConcurrentBuilds()
    }

    stages {
        stage('Git Clone') {
            steps {
                sh '''
                    if [ ! -d .git ]; then
                        git clone https://github.com/pro-digital-uk/team-alpha-infra.git .
                        git checkout ${BRANCH}
                    else
                        git fetch origin
                        git checkout ${BRANCH}
                        git pull origin ${BRANCH}
                    fi
                '''
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
