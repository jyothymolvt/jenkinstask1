pipeline {
    agent any

    environment {
        AWS_REGION = 'ap-south-1'
        AWS_ACCOUNT_ID = '344548866539'
        IMAGE_NAME = 'myproject'
        REPO_URL = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${IMAGE_NAME}"
    }

    stages {
        stage('stage1-Create index.php') {
            steps {
                sh '''#!/bin/bash
cat <<EOF > index.php
<?php
echo "<h1>Welcome to My Sample PHP Page!</h1>";
echo "<p>This is a sample Dockerized PHP application.</p>";
?>
EOF

                echo "Index file created:"
                cat index.php
                '''
            }
        }
        stage('stage2-create Dockerfile') {
            steps {
                  sh '''#!/bin/bash
cat <<EOF > Dockerfile
 # Use the official PHP image with Apache
FROM php:8.1-apache

# Copy the PHP file to the Apache web directory
COPY index.php /var/www/html/

# Expose port 80
EXPOSE 80

# Start Apache in the foreground
CMD ["apache2-foreground"]
EOF
                echo "Dockerfile created:"
                ls -la
                cat Dockerfile
                 '''
                }
           }
      stage('stage-3')  {
     steps {
            withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws-creds']]) {
                    sh '''
                    aws ecr get-login-password --region $AWS_REGION | \
                    docker login --username AWS --password-stdin $REPO_URL

                    docker build -t $IMAGE_NAME .
                    docker tag $IMAGE_NAME:latest $REPO_URL:latest
                    docker push $REPO_URL:latest
                    '''
              }
            }
        }   
    }
 }
