pipeline {
    agent any
    tools {
            maven 'maven-for-jenkins'
    }
    stages {
        stage ('Build Maven') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Java-Techie-jt/devops-automation']]])
                sh 'mvn clean install'
            }
        }
        stage('Build docker image') {
            steps {
                script {
                    sh 'docker build -t sahilpabale/devops-integration .'
                }
            }
        }
        stage('Push image to hub') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'dockerhubpwd', variable: 'dockerhubpwd')]) {
                        sh 'docker login -u sahilpabale -p ${dockerhubpwd}'
                    }

                    sh 'docker push sahilpabale/devops-integration'
                }
            }
        }
    }
}