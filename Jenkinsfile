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
                        -Dsonar.projectKey=Java-App \
                        -Dsonar.host.url=http://51.21.1.51:9000
                    '''
                }
            }
        }

        stage('Dependency Check') {
            steps {
                dependencyCheck additionalArguments: '--scan .',
                odcInstallation: 'DependencyCheck'
                dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }
    }
}