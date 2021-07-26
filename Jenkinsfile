pipeline {
    agent any 
    
    tools {
        maven 'M3'
    }
    
    stages {
        stage('Cleaning workspace'){
            steps{
                sh 'echo "---=--- Cleaning Stage ---=---"'
                sh 'mvn clean'
                script {
                        try{
                     sh 'docker stop myboot && docker rm myboot'
                    }catch (Exception e){
                      sh 'echo "---=--- No container to remove ---=---"'
                    }
                }
            }
        }
        stage('Checkout'){
            steps{
                sh 'echo "---=--- Checkout ---=---"'
                git branch: 'main', url: 'https://github.com/RomainD93/simple-boot.git'
            }
        }
        stage('Compile'){
            steps{
                sh 'echo "---=--- Compile ---=---"'
                sh 'mvn compile'
            }
        }
        stage('Test'){
            steps{
                sh 'echo "---=--- Test ---=---"'
                sh 'mvn test'
            }
        }
        stage('Package'){
            steps{
                sh 'echo "---=--- Package ---=---"'
                sh 'mvn package -DskipTests'
            }
        }
        stage('Docker'){
            steps{
                sh 'echo "---=--- Docker ---=---"'
                sh 'docker build -t romaind93/myboot:1.0 .'
            }
        }
        stage('Deploy to Local'){
            steps{
                sh 'echo "---=--- Deploy to Local ---=---"'
                sh 'docker run -d --name myboot -p 9393:9393 romaind93/myboot:1.0'
            }
        }
    }
}