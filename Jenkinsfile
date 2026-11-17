pipeline {
    agent {
        label "Agent-01"
    }
    tools {
        jdk 'jdk-11'
        maven 'maven-3-5-4'
    }
    stages {
        stage('Build Java app') {
            steps {
                sh "mvn package install -DskipTests=true"
            }
        }
        stage('Test Java app') {
            steps {
                sh "mvn test"
            }
        }
        stage('Bulid Docker image') {
            steps {
                sh "docker bulid -t javaapp:v1 ."
            }
        }
    }
}