//Create a Jenkinsfile under the project
pipeline {
    agent any

    parameters {
        choice(
            name: 'ENV',
            choices: ['dev', 'qa', 'prod'],
            description: 'Select environment to deploy'
        )
    }

    environment {
        APP_NAME = "my-java-app"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/company/my-java-app.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Deploy') {
            steps {
                script {
                    if (params.ENV == 'dev') {
                        sh './deploy.sh dev'
                    }
                    else if (params.ENV == 'qa') {
                        sh './deploy.sh qa'
                    }
                    else if (params.ENV == 'prod') {
                        sh './deploy.sh prod'
                    }
                }
            }
        }
    }

    post {
        success {
            echo "Deployment successful to ${params.ENV}"
        }

        failure {
            echo "Build or deployment failed"
        }
    }
}