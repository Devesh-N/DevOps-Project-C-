pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/Devesh-N/DevOps-Project-C-Sharp.git', branch: 'main'
            }
        }
        stage('Restore') {
            steps {
                sh 'dotnet restore'
            }
        }

        stage('Build') {
            steps {
                sh 'dotnet build --configuration Release'
            }
        }

        stage('Test') {
            steps {
                sh 'dotnet test --no-build --configuration Release'
            }
        }

        stage('Publish') {
            steps {
                sh 'dotnet publish --configuration Release --output ./publish'
                sh 'whoami'
            }
        }

        stage('Deploy to Production') {
            steps {
                script {
                    echo '***************************'
                    echo "Deploying to production..."
                    echo '***************************'
                    sh 'pwd'
                    sh '''
                        ssh -o StrictHostKeyChecking=no -i /var/jenkins_home/workspace/stage.pem ubuntu@ec2-13-126-238-41.ap-south-1.compute.amazonaws.com '
                            sudo su
                            cd /app
                            sudo rm -r *
                            sudo git clone https://github.com/Devesh-N/DevOps-Project-C-Sharp.git
                            cd DevOps-Project-C-Sharp
                            sudo dotnet restore
                            sudo dotnet build --configuration Release
                            sudo dotnet test --no-build --configuration Release
                            sudo dotnet publish --configuration Release --output ./publish
                        '
                    '''
                    // Example: sh './deploy-to-production.sh'
                }
            }
        }
    }
}
