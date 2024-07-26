pipeline {
    agent any

    stages {
        stage('Drop the Apache HTTPD Docker container'){
            steps {
            echo 'droping the container...'
            sh 'docker rm -f apache1'
            }
        }
        stage('Create the Apache httpd container') {
            steps {
            echo 'Creating the container...'
            sh 'docker run -dit --name apache1 -p 9000:80  -v /tmp/web:/usr/local/apache2/htdocs/ httpd'
            }
        }
        stage('Copy the web application to the container directory') {
            steps {
                echo 'Copying web applucation...'             
                sh 'cp -r web/* /tmp/web'
            }
        }
        stage('Checking the app') {
            steps {
                echo 'Testing the web app'
                sh 'wget http://localhost:9000'
            }
        }       
    }
}
