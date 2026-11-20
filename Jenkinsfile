@Library('sharedlib-test') _

pipeline {
    agent { 
        label 'Agent-01' 
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
                script {
                    def mvn = new edu.depi.maven()
                    mvn.MavenCommand("package install -DskipTests=true")
                }
            }
        }

        stage('Test Java app') {
            steps{
                script {
                    def mvn = new edu.depi.maven()
                    mvn.MavenCommand("test")
                }
            }
        }

        stage('Bulid Docker image'){
            steps{
                script {
                    def dockerfun = new edu.depi.docker()
                    dockerfun.DockerBulid( "${dockerhub_username}/java-app","v40")
                }
            }
            
        }
        stage('Doker Login') {
            steps{
                script {
                    def dockerfun = new edu.depi.docker()
                    dockerfun.DockerLogin("${dockerhub_username}","${dockerhub_password}")
                }
            }
        }
        // stage('Push Docker image') {
        //     steps {
        //         sh "docker push ${dockerhub_username}/java-app:v${BUILD_NUMBER}"
        //     }
        // }
        stage('Deploy Docker image') {
            agent { 
                label 'Agent-02' 
            }
            steps {
                sh '''
                IMAGE_FOUNDED=$(docker ps -a | grep java-app) || true
                if [[ -n $IMAGE_FOUNDED ]]; 
                then docker rm -f java-app && docker run -d -p 8090:8090 --name java-app ${dockerhub_username}/java-app:v${BUILD_NUMBER};
                else docker run -d -p 8090:8090 --name java-app ${dockerhub_username}/java-app:v${BUILD_NUMBER};
                fi
                '''
            }
        }
    }
}