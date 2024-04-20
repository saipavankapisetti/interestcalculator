pipeline {
    agent any
    
    triggers {
        pollSCM('* * * * *') // Poll SCM every minute
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from GitHub repository
                git branch: 'main', url: 'https://github.com/saipavankapisetti/interestcalculator.git'
            }
        }

        stage('Nginx Installation') {
            steps {
                script {
                    try {
                        // Install Nginx
                        sh 'sudo apt update && sudo apt install nginx -y'

                        // Start Nginx
                        sh 'sudo systemctl start nginx'

                        // Check Nginx status
                        sh 'sudo systemctl status nginx'
                    } catch (Exception e) {
                        currentBuild.result = 'FAILED'
                        error "Failed to install and start Nginx: ${e.message}"
                    }
                }
            }
        }

        stage('Move Index File') {
            steps {
                // Move index.html file to /var/www/html/index.nginx-debian.html with sudo permissions
                script {
                    try {
                        sh 'sudo mv index.html /var/www/html/index.nginx-debian.html'
                        currentBuild.result = 'SUCCESS'
                    } catch (Exception e) {
                        echo "Failed to move index.html: ${e.message}"
                        currentBuild.result = 'FAILURE'
                        error "Failed to move index.html"
                    }
                }
            }
        }
    }

    post {
        success {
            echo "Pipeline completed successfully!"
            // Additional actions to take upon successful completion
        }
        failure {
            echo "Pipeline failed!"
            // Additional actions to take upon failure
        }
    }
}
