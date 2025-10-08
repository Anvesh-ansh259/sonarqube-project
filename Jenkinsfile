pipeline {
    agent { label 'sonarqube-node' } // Make sure your agent label matches

    environment {
        SONAR_SCANNER_HOME = '/usr/local/lib/node_modules/sonar-scanner' // Path where npm installed sonar-scanner
        SONAR_HOST_URL = 'http://3.80.136.249:9000'                        // Your SonarQube server
        SONAR_LOGIN = 'sonartoken'                                         // Your SonarQube token
    }

    stages {
        stage('Checkout') {
            steps {
                git(
                    url: 'https://github.com/Anvesh-ansh259/sonarqube-project.git',
                    branch: 'main',
                    credentialsId: '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'
                )
            }
        }

        stage('Install Dependencies') {
            steps {
                // Wrap long-running npm install in timeout
                timeout(time: 20, unit: 'MINUTES') {
                    sh 'npm install --verbose'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') { // Name from Manage Jenkins → SonarQube servers
                    sh '''
                        sonar-scanner \
                        -Dsonar.projectKey=my_first_sonarqube_project \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=$SONAR_HOST_URL \
                        -Dsonar.login=$SONAR_LOGIN
                    '''
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 10, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }

    post {
        success {
            echo 'Build and SonarQube analysis completed successfully!'
        }
        failure {
            echo 'Build failed. Check console output for details.'
        }
    }
}
