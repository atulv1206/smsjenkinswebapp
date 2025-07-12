pipeline {
    agent any

    environment {
        DOTNET_ROOT = "C:\\Program Files\\dotnet"
        PATH = "${env.DOTNET_ROOT};${env.PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                bat "dotnet restore"
                bat "dotnet build --configuration Release"
            }
        }

        stage('Test') {
            steps {
                bat "dotnet test --no-restore --configuration Release"
            }
        }

        stage('Publish') {
            steps {
                // Clean up old publish folder to avoid conflicts
                bat "rmdir /S /Q publish || echo publish folder does not exist"
                // Publish to local folder
                bat "dotnet publish --no-restore --configuration Release --output publish"
            }
        }

        stage('Deploy') {
            steps {
                // Copy to network share (IIS folder)
                bat 'xcopy /E /Y /Q publish\\* \\\\DEEPIKA\\coreapp\\'
            }
        }
    }

    post {
        success {
            echo '✅ Build, test, publish, and deploy succeeded!'
        }
        failure {
            echo '❌ Build or deploy failed.'
        }
    }
}
