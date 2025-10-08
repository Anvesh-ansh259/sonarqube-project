pipeline {
    agent { label 'sonarqube-node' } // your agent

    environment {
        // SonarQube token and server URL
        SONAR_HOST_URL = 'http://3.80.136.249:9000'
        SONAR_TOKEN = 'sonartoken'  // replace with your actual token
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/Anvesh-ansh259/sonarqube-project.git', branch: 'main'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                sh """
                    sonar-scanner \
                    -Dsonar.projectKey=my_first_sonarqube_project \
                    -Dsonar.projectName="My First SonarQube Project" \
                    -Dsonar.projectVersion=1.0 \
                    -Dsonar.sources=. \
                    -Dsonar.language=js \
                    -Dsonar.host.url=$SONAR_HOST_URL \
                    -Dsonar.login=$SONAR_TOKEN
                """
            }
        }
    }

    post {
        success {
            echo "SonarQube analysis completed successfully!"
        }
        failure {
            echo "Build failed or SonarQube analysis failed."
        }
    }
}
