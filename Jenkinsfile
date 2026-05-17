pipeline {
    agent none
    stages {
        stage('Maven Install') {
            agent {
                docker {
                    image 'maven:3.9-eclipse-temurin-25'
                    reuseNode true
                }
            }
            steps {
                sh 'mvn clean install -DskipTests'
            }
        }
        stage('Docker Build') {
            agent any
            steps {
                sh 'docker build -t mariofer2001/spring-petclinic:gestion-udem-jenkins .'
            }
        }
        stage('Docker Push') {
            agent any
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerHub', usernameVariable: 'dockerHubUser', passwordVariable: 'dockerHubPassword')]) {
                    sh "docker login -u \${env.dockerHubUser} -p \${env.dockerHubPassword}"
                    sh 'docker push mariofer2001/spring-petclinic:gestion-udem-jenkins'
                }
            }
        }
    }
}
