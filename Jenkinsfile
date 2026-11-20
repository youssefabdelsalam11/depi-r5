pipeline {
    agent none
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
            agent { 
                label 'Agent-01' 
            }
            steps { 
                sh " mvn package install -DskipTests "
            }
        }
        stage('Test Java app') {
            agent { 
                label 'Agent-01' 
            }
            steps {
                sh "mvn test"
            }
        }
        stage('Bulid Docker image'){
            agent { 
                label 'Agent-01' 
            }
            steps {
                sh"docker build -t ${dockerhub_username}/java-app:v${BUILD_NUMBER} ."
            }
        }
        stage('Doker Login') {
            agent { 
                label 'Agent-01' 
            }
            steps {
                sh "docker login -u ${dockerhub_username} -p ${dockerhub_password}"
            }
        }
        stage('Deploy Docker image') {
            agent { 
                label 'Agent-02' 
            }
            steps {
                sh '''
                IMAGE_FOUNDED=$(docker ps -a | grep java-app)  
                if [[-n IMAGE_FOUNDED]]; 
                then docker rm -f java-app && docker run -d -p 8090:8090 --name java-app ${dockerhub_username}/java-app:v26;
                else docker run -d -p 8090:8090 --name java-app ${dockerhub_username}/java-app:v26;
                fi
                '''
            }
        }
    }
}