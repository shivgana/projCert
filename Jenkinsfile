pipeline {
    agent any
    stages {
        stage('Install and configure Puppet agent') {
            steps {
                // run shell commands to install and configure Puppet agent on the slave node
                sh 'wget https://apt.puppetlabs.com/puppet6-release-focal.deb'
                sh 'sudo dpkg -i puppet6-release-focal.deb'
                sh 'sudo apt-get update -y '
                sh 'sudo apt-get install -y puppet-agent'
                sh 'puppet config set server 192.168.251.1'
                sh 'puppet agent -t'
            }
        }
        stage('Push Ansible configuration to install Docker') {
            steps {
                // run Ansible playbook to install Docker on test server
                ansiblePlaybook playbook: 'setup_docker.yaml'
            }
        }
        stage('Build and deploy PHP Docker container') {
            steps {
                // clone git repo containing PHP website and Dockerfile
                git url: 'https://github.com/shivgana/projCert.git'
                // build and tag Docker container
                sh 'docker build -t eduphpproj .'
                // run Docker container
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
