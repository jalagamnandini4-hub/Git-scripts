pipeline {
    agent any

    environment {
        IMAGE_NAME = 'nandini345/jmetertesting:v1.0'  // Your Docker image name
        TEST_REPO = 'https://github.com/jalagamnandini4-hub/Git-scripts.git'  // Your GitHub repo URL
    }

    stages {
        stage('Checkout Tests') {
            steps {
                // Clone your test scripts repository with credentials
                git branch: 'main', credentialsId: "${params.gitCredentialsID}", url: "${TEST_REPO}"
            }
        }
        
        stage('Run JMeter Tests') {
            steps {
                // Use bat instead of sh and adjust volume mount for Windows PowerShell syntax
                bat """
                docker run --rm -v %cd%\\JMX_Files:/tests -w /tests ${IMAGE_NAME} ^
                -n -t AB_ACCOUNT_SUMMARY.jmx -l AB_ACCOUNT_SUMMARY_results.jtl
                """
            }
        }

        stage('Archive Results') {
            steps {
                archiveArtifacts artifacts: 'AB_ACCOUNT_SUMMARY_results.jtl, report/**', allowEmptyArchive: true
            }
        }
    }
}
