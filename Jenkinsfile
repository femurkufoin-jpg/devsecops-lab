pipeline {
    agent any

    tools {
        maven 'maven3'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('SonarQube Scan') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh '''
                        mvn sonar:sonar \
                        -Dsonar.projectKey=java-app \
                        -Dsonar.host.url=http://51.21.1.51:9000
                    '''
                }
            }
        }

        stage('Dependency Check') {
            steps {
                withCredentials([string(credentialsId: 'nvd-api-key', variable: 'NVD_API_KEY')]) {

                    dependencyCheck(
                        odcInstallation: 'DependencyCheck',
                        additionalArguments: "--scan . --nvdApiKey ${NVD_API_KEY} --nvdApiDelay 5000",
                    )

                    dependencyCheckPublisher(
                        pattern: '**/dependency-check-report.xml'
                    )
                }
            }
        }
    }
}