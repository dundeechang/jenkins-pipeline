pipeline {
    agent none
    stages {
        stage('Build') {
            agent {
                label 'cpp'
            }
            steps {
                echo 'Building..'
                git branch: 'gtest', url: 'https://github.com/dundeechang/helloworld.cpp.git'
                sh 'g++ helloworld.cpp -o helloworld'
                sh 'g++ unittest.cpp -o unittest -Igtest/include -Lgtest/lib -lgtest -lpthread'
            }
            post {
                always {
                    lastChanges format:'SIDE', matching: 'LINE'
                }
                success {
                    archiveArtifacts artifacts: 'helloworld,unittest'
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
                curl -O ${BUILD_URL}artifact/unittest
                chmod +x unittest
                ./unittest --gtest_output="xml:report.xml"
                '''
            }
            post {
                always {
                    junit '*.xml'
                }
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}