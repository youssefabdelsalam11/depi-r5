pipeline {
    agent {
        label "Agent-01"
    }
    tools {
        jdk 'jdk-11'
        maven 'maven-3-5-4'
    }
    environment {
        dockerhub_username = credentials("dockerhub-username")
        dockerhub_password = credentials("dockerhub-password")
    }
    stages {
        stage('Build Java app') {
            steps { 
                sh " mvn package install -DskipTests "
            }
        }
        stage('Test Java app') {
            steps {
                sh "mvn test"
            }
        }
        stage('Doker Login') {
            steps {
                sh 'docker login -u "$dockerhub_username" -p "$dockerhub_password"'
            }
        }

        stage('Build and Push Docker image') {
            steps {
                
                sh "docker build -t ${dockerhub_username}/java-app:${BUILD_NUMBER} ."

                sh "docker push ${dockerhub_username}/java-app:${BUILD_NUMBER}"
            }
        }
    }
}