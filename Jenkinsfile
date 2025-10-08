pipeline {
    agent { label 'sonarqube-node' } // your agent label

    environment {
        SONAR_TOKEN = credentials('SONAR_TOKEN') // Jenkins credential for admin token
        SONAR_HOST = 'http://3.80.136.249:9000'  // SonarQube server URL
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
                    // Create directory for compiled classes
                    sh 'mkdir -p target/classes'
                    // Compile Java source files
                    sh 'javac -d target/classes src/main/java/*.java'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh """
                        sonar-scanner \
                        -Dsonar.projectKey=JavaAppTest \
                        -Dsonar.projectName=JavaAppTest \
                        -Dsonar.sources=src/main/java \
                        -Dsonar.java.binaries=target/classes \
                        -Dsonar.host.url=${SONAR_HOST} \
                        -Dsonar.token=${SONAR_TOKEN}
                    """
                }
            }
        }

        stage('Quality Gate') {
            steps {
                // Wait up to 10 minutes for Quality Gate result
                timeout(time: 10, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully! ✅'
        }
        failure {
            echo 'Pipeline failed! ❌ Check SonarQube report.'
        }
    }
}
