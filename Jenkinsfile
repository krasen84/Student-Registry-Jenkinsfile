pipeline {
    agent any

    stages {
        stage('Repo checkout') {
            steps {
                checkout scm
            }
        }
        stage('NPM Install') {
            steps {
                bat "npm install"
            }
        }
        stage('Run Integration Tests') {
            steps {
                bat "npm run test"
            }
        }
        stage('Build Image') {
            steps {
                
                withCredentials([usernamePassword(credentialsId: 'd6c01c79-d2f2-4016-a697-7ccf2202ffa4', passwordVariable: 'pass', usernameVariable: 'user')]) {
                    bat """docker build -t krasen84/student:1.0.0 .
                        docker login --username %user% --password %pass% 
                        docker push krasen84/student:1.0.0"""
                }
            }
        }
        stage('Deploy Image') {
            steps {
                
                withCredentials([usernamePassword(credentialsId: 'd6c01c79-d2f2-4016-a697-7ccf2202ffa4', passwordVariable: 'pass', usernameVariable: 'user')]) {
                    bat """docker pull krasen84/student:1.0.0 
                        docker run -d -p 8081:8081 krasen84/student:1.0.0"""
                }
            }
        }
    }
}
