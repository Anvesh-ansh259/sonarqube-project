pipeline {
    agent any

    environment {
        // SonarQube server defined in Jenkins -> Configure System -> SonarQube servers
        SONARQUBE_ENV = 'SonarQube'
        // Jenkins credential containing the SonarQube admin token
        SONAR_TOKEN = credentials('SONAR_TOKEN')
    }

    stages {
        stage('Checkout SCM') {
            steps {
                git(
                    url: 'https://github.com/Anvesh-ansh259/sonarqube-project.git',
                    branch: 'main',
                    credentialsId: 'github-pat'
                )
            }
        }

        stage('Compile Java') {
            steps {
                sh 'mkdir -p target/classes'
                sh 'javac -d target/classes src/main/java/*.java'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv(SONARQUBE_ENV) {
                    sh """
                        sonar-scanner \
                        -Dsonar.projectKey=JavaAppTest \
                        -Dsonar.projectName=JavaAppTest \
                        -Dsonar.sources=src/main/java \
                        -Dsonar.java.binaries=target/classes \
                        -Dsonar.host.url=${env.SONAR_HOST_URL} \
                        -Dsonar.token=${SONAR_TOKEN}
                    """
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 10, unit: 'MINUTES') {
                    // Wait for SonarQube Quality Gate result
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline completed successfully.'
        }
        failure {
            echo '❌ Pipeline failed! Check SonarQube report.'
        }
    }
}
