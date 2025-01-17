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
                    bat "dotnet build --configuration Release /p:Environment=Development"
                }
            }
        }

        stage('Test'){
            steps {
                script {
                    bat "dotnet test --no-restore --configuration Release /p:Environment=Development"
                }
            }
        }

        stage('Publish'){
            steps {
                script {
                    bat "dotnet publish --no-restore --configuration Release /p:Environment=Development --output .\\publish"
                }
            }
        }

         stage('Deploy'){
            steps {
                script {
                   withCredentials([usernamePassword(credentialsId:'coreuser', passwordVariable:'CREDENTIAL_PASSWORD', usernameVariable:'CREDENTIAL_USERNAME')]){
                    powershell '''
                    
                    $password = ConvertTo-SecureString $env:CREDENTIAL_PASSWORD -AsPlainText -Force
                    $credentials = New-Object System.Management.Automation.PSCredential($env:CREDENTIAL_USERNAME, $password)
                    
                    New-PSDrive -Name X -PSProvider FileSystem -Root "\\\\INEXA-DEV-PC57\\jk_webapi" -Persist -Credential $credentials

                    Copy-Item -Path 'publish\\*' -Destination 'X:\' -Force

                    Remove-PSDrive -Name X
                    '''
                   }
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