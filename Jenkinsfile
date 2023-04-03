pipeline {
    agent any
    
    stages {
        stage('Clone Git repo') {
            steps {
                git branch: 'master', url: 'https://github.com/shivgana/projCert.git'
            }
        }
        stage('Install and configure puppet agent on the slave node') {
            steps {
                // code for Job 1
            }
        }
        
        stage('Push Ansible configuration to install docker on test server') {
            steps {
                sh 'ansible-playbook setup_docker.yaml'
            }
        }
        
        stage('Build and deploy PHP Docker container') {
            agent slave
            steps {
                sh 'docker build -t docshiva/eduphpproj .'
                sh 'docker run -d php-app docshiva/eduphpproj'
            }
        }
        
        stage('Push Docker image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', passwordVariable: 'DOCKERHUB_PASSWORD', usernameVariable: 'DOCKERHUB_USERNAME')]) {
                    sh 'docker login -u $DOCKERHUB_USERNAME -p $DOCKERHUB_PASSWORD'
                    sh 'docker push docshiva/eduphpproj:latest'
                }
            }
        }
        
        stage('Delete running container on Test Server if Job 3 fails') {
            steps {
                // code for Job 4
            }
        }
    }
}
