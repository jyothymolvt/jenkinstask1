pipeline {
    agent any

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
            sh '''
            aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 344548866539.dkr.ecr.ap-south-1.amazonaws.com
            docker build -t myproject .
            docker tag myproject:latest 344548866539.dkr.ecr.ap-south-1.amazonaws.com/myproject:latest
            docker push 344548866539.dkr.ecr.ap-south-1.amazonaws.com/myproject:latest
            '''

            }
        }   
    }
 }
