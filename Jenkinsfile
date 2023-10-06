pipeline {

    environment {
        dockerimagename = "ostock/configserver:0.0.1-SNAPSHOT"
        dockerImage = ""
    }

    agent any

    tools {
        maven 'tuna-maven'
    }
    stages {
        stage('Build with Maven') {
            steps {
                sh 'mvn --version'
                sh 'java -version'
                sh 'mvn clean package -Dmaven.test.failure.ignore=true'
            }
        }

        stage('Build image') {
            steps{
                script {
                  dockerImage = docker.build dockerimagename
                }
            }
        }

        stage('Pushing Image') {
            steps {
                withDockerRegistry(credentialsId: 'dockerhub', url: 'https://index.docker.io/v1/') {
                    sh 'docker push ostock/configserver:0.0.1-SNAPSHOT'
                }
            }
        }

        stage('Deploying container to Kubernetes') {
            steps {
                script {
                  kubernetesDeploy(configs: "configserver-deployment.yaml", "configserver-service.yaml")
                }
            }
        }

    }
}