pipeline {
    agent any

    triggers {

        pollSCM '* * * * *'
        cron '* * * * *'
    }

    parameters {

        choice choices:["nginx", "apache"], name: "SERVER"
        choice choices:["dev", "test", "prod"], name: "ENV"
    }

  
    stages {
        stage('Create web directory')
        {
            when {
                equals(actual: currentBuild.number, expected: 1)
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
                allOf{
                    
                    environment name: "SERVER", value: "apache"
                    environment name: "ENV", value: "dev"
                }
            }
            steps {
            echo 'Creating the container...'
            sh 'docker run --privileged -dit --name apache1 -p 9100:80  -v /home/rafaelpolo/Documentos/Estudio/install/Jenkins/web:/usr/local/apache2/htdocs/ httpd'
            }
        }
        
        stage('Create the Apache nginx container') {
            when {
                allOf{
                    
                    environment name: "SERVER", value: "nginx"
                    environment name: "ENV", value: "test"
                }            
            }
            
            steps {
            echo 'Creating the container...'
            sh 'docker run --privileged -dit --name apache1 -p 9100:80  -v /home/rafaelpolo/Documentos/Estudio/install/Jenkins/web:/usr/share/nginx/html nginx'
            }
        }

        stage('control error'){
            when{
                allOf{
                    environment name: "SERVER", value: "apache"
                    environment name: "ENV", value: "test"
                }
            }
            steps{
                echo "no puede desplegar apache en test"
            }
        }
        stage('control error2'){
            when{
                allOf{
                    environment name: "SERVER", value: "nginx"
                    environment name: "ENV", value: "dev"
                }
            }
            steps{
                echo "no puede desplegar nginx en dev"
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
