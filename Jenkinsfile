pipeline {
    agent { label 'sonarqube-node' } // your agent

    environment {
        SONAR_TOKEN = credentials('SONAR_TOKEN') // add your SonarQube token in Jenkins credentials
        SONAR_HOST = 'http://3.80.136.249:9000' // your SonarQube server URL
    }

    stages {
        stage('Checkout SCM') {
            steps {
                checkout([
                    $class: 'GitSCM', 
                    branches: [[name: '*/main']], 
                    userRemoteConfigs: [[
                        url: 'https://github.com/Anvesh-ansh259/sonarqube-project.git',
                        credentialsId: 'github-pat'
                    ]]
                ])
            }
        }

        stage('Compile Java') {
            steps {
                script {
                    // Create a directory for compiled classes
                    sh 'mkdir -p target/classes'
                    // Compile Java files
                    sh 'javac -d target/classes src/main/java/*.java'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh """
                        sonar-scanner \
                        -Dsonar.projectKey=JavaApp
