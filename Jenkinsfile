pipeline {
    agent any
    options {
        timeout(time: 20, unit: 'MINUTES')
    }
    stages{
        // NPM dependencies
        stage('pull npm dependencies') {
            steps {
                sh 'cd app && npm install'
            }
        }
        // Run Unit test
        stage('Run Unit Test') {
            steps {
                sh 'cd app && npm test'
            }
        }
        // run sonarqube test
        stage('Run Sonarqube') {
            environment {
                scannerHome = tool 'ibt-sonarqube';
            }
            steps {
              withSonarQubeEnv(credentialsId: 'SQ-student', installationName: 'IBT sonarqube') {
                sh "${scannerHome}/bin/sonar-scanner"
              }
            }
        }
    }
}
