pipeline {
    agent any
   
    stages {
        stage('Configure Docker remote context') {
            steps {
                echo 'Configuring Docker remote context...'
                sh '''
                  docker context inspect remoto >/dev/null 2>&1 || \
                  docker context create remoto --docker "host=ssh://pi@192.168.2.4"
                  docker context use remoto
                  docker context ls
                '''
            }
        }
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
                //Who is executing this process
                sh 'whoami'
                //Check pwd folder
                sh 'pwd'
                //Check Workspace
                echo "Workspace path: ${env.WORKSPACE}"        
            }
        }
        stage('Drop the Apache HTTPD Docker container'){
            steps {
            echo 'droping the container...'
            sh 'docker rm -f apache1'
            }
        }
        stage('Create the Apache httpd container') {
            steps {
                echo 'Creating the container...'
                sh '''
                  docker run -dit \
                    --name apache1 \
                    -p 9001:80 \
                    -v $WORKSPACE/web:/usr/local/apache2/htdocs/ \
                    httpd
                '''
    }
}    
    }
}
