pipeline {
    agent none
    stages {
        stage('Build') {
            agent {
                label 'cpp'
            }
            steps {
                echo 'Building..'
                git 'https://github.com/dundeechang/helloworld.cpp.git'
                sh 'g++ helloworld.cpp -o helloworld'
            }
            post {
                success {
                    archiveArtifacts artifacts: 'helloworld'
                }
            }
        }
        stage('Test') {
            agent {
                label 'test'
            }
            steps {
                echo 'Testing..'
                sh '''#!/bin/bash
                curl -O ${BUILD_URL}artifact/helloworld
                chmod +x helloworld
                ./helloworld
                '''
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}