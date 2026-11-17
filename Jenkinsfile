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
                sh " mvn package install -DskipTests "
            }
        }
        stage('Test Java app') {
            steps {
                sh "mvn test"
            }
        }
        stage('Build Docker image') {
            steps {
                sh "docker build -t javaapp:v1 ."
            }
        }
    }
}