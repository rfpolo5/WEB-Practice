pipeline {
    agent any

    environment {

        SERVER = "nginx"
    
    }
   
    stages {
        stage('Create web directory')
        {
            input {
              message 'Enter the data'
              parameters {
                    string(name:'AUTHOR', defaultValue: 'Sergio', description: 'Author of the web application deployment ')
                    string(name:'ENVIRONMENT', defaultValue: 'Development',description: 'Environment to deploy')
                 }
            }
            steps{
                echo "The responsible of this project is ${AUTHOR} and and will be deployed in ${ENVIRONMENT}"
                //Fisrt, drop the directory if exists
                sh 'rm -rf /home/rafaelpolo/Documentos/Estudio/install/Jenkins/web'
                //Create the directory
                sh 'mkdir /home/rafaelpolo/Documentos/Estudio/install/Jenkins/web'
                
            }
        }
        stage('Drop the Apache HTTPD Docker container'){
            steps {
            echo 'droping the container...'
            sh 'docker rm -f apache1'
            }
        }
        stage('Create the Apache httpd container') {
            when { 
                environment name: "SERVER", value: "apache"
            }
            steps {
            echo 'Creating the container...'
            sh 'docker run --privileged -dit --name apache1 -p 9100:80  -v /home/rafaelpolo/Documentos/Estudio/install/Jenkins/web:/usr/local/apache2/htdocs/ httpd'
            }
        }
        
        stage('Create the Apache nginx container') {
            when {

                environment name: "SERVER", value: "nginx"
            }
            
            steps {
            echo 'Creating the container...'
            sh 'docker run --privileged -dit --name apache1 -p 9100:80  -v /home/rafaelpolo/Documentos/Estudio/install/Jenkins/web:/usr/share/nginx/html nginx'
            }
        }
        
        stage('Copy the web application to the container directory') {
            steps {
                echo 'Copying web application...'             
                sh 'cp -r web/* /home/rafaelpolo/Documentos/Estudio/install/Jenkins/web'
            }
        }
        stage('Checking the app') {
            steps {
                echo 'Testing the web app'
                sh 'wget http://localhost:9100'
            }
        }       
    }
}
