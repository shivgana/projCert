pipeline {
    agent any
    stages {
        stage('Install and configure Puppet agent') {
            steps {
                sh 'wget https://apt.puppetlabs.com/puppet6-release-focal.deb'
                sh 'sudo dpkg -i puppet6-release-focal.deb'
                sh 'sudo apt-get update -y '
                sh 'sudo apt-get install -y puppet-agent'
                //sh 'puppet config set server 192.168.251.1'
                //sh 'puppet agent -t'
            }
        }
        stage('Push Ansible configuration to install Docker') {
            steps {
                ansiblePlaybook playbook: 'setup_docker.yaml'
            }
        }
        stage('Build and deploy PHP Docker container') {
            steps {
                git url: 'https://github.com/shivgana/projCert.git'
                sh 'docker build -t eduphpproj .'
                sh 'docker run -d -p 80:80 eduphpproj'
            }
            post {
                failure {
                    // if previous stage fails, delete running container on test server
                    sh 'docker rm -f $(docker ps -aq)'
                }
            }
        }
    }
}
