pipeline {
    agent any

    environment {
        EC2_USER = 'ubuntu' 
        EC2_HOST = 'ec2-13-203-75-85.ap-south-1.compute.amazonaws.com'
        PEM_FILE = 'C:\Users\I\Downloads/mykey1.pem' // path to your private key on Jenkins
        APP_DIR = '/home/ubuntu/myapp' // where to deploy the app
    }

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/manojac/c.project.git'
            }
        }

        stage('Build') {
            steps {
                sh './build.sh' // or any build commands, e.g., `npm install`, `mvn package`
            }
        }

        stage('Deploy to EC2') {
            steps {
                sh '''
                    echo "Copying files to EC2..."
                    scp -o StrictHostKeyChecking=no -i $PEM_FILE -r * $EC2_USER@$EC2_HOST:$APP_DIR

                    echo "Restarting application on EC2..."
                    ssh -o StrictHostKeyChecking=no -i $PEM_FILE $EC2_USER@$EC2_HOST << EOF
                        cd $APP_DIR
                        ./restart.sh
                    EOF
                '''
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}

