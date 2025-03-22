pipeline{
    agent {label "master"}
    environment {
        SHELL="/bin/bash"
    }
    stages{
        stage("Debug Docker") {
            steps {
                sh "whoami"
                sh "id"
                sh "env"
                sh "which docker || echo 'Docker not found'"
                sh "docker --version || echo 'Docker command not available'"
            }
        }    
         stage('init environment') {
            steps {
                echo "initializing the environment"
                sh 'export PATH=$PATH:/usr/bin'
            }
        }
        stage("SCM"){
            steps{
                git branch: 'main', url: 'https://github.com/pranaygupta1988/website.git'
            }
        }
        stage("Build Docker Image"){
            steps{
                sh "docker image build -t pranaygupta1988/website1 ."
            }
        }
        stage("Docker Login"){
            steps{
                withCredentials([string(credentialsId: 'DOCKER_HUB_TOKEN', variable: 'DOCKER_HUB_TOKEN')]) {
                sh "echo $DOCKER_HUB_TOKEN | docker login -u pranaygupta1988 --password-stdin"
                 }
            }
        }
        stage("Push Docker Image "){
            steps{
                sh "docker image push pranaygupta1988/website1"
            } 
        }
        stage("Remove Existing Docker Service"){
            steps{
                sh "docker service rm website"
            }
        }
        stage("Create Docker Service"){
            steps{
                sh "docker service create --name website --replicas 5 -p 8090:80 pranaygupta1988/website1"
            }
        }
    }
}
