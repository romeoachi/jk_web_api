pipeline {
    agent any

    environment {
        DONET_CLI_HOME = "C:\\Program Files\\dotnet"
    }

    stages {
        stage('checkout'){
            steps {
                checkout scm
            }
        }

        stage('build') {
            steps {
                script {
                    bat "dotnet restore"
                    bat "dotnet build --configuration Release"
                }
            }
        }

        stage('Test'){
            steps {
                script {
                    bat "dotnet test --no-restore --configuration Release"
                }
            }
        }

        stage('Publish'){
            steps {
                script {
                    bat "dotnet publish --no-restore --configuration Release --output .\\publish"
                }
            }
        }
    }

    post {
        success{
            echo 'Build, test, and publish successful'
        }
    }
}