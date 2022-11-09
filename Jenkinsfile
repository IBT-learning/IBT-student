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
        stage('build Docker Container') {
            steps {
                script {
                    // build image
                    docker.build("630437092685.dkr.ecr.us-east-2.amazonaws.com/ibt-student:latest")
                }
            }
        }
        stage('Push to ECR') {
            steps {
                script{
                    //https://<AwsAccountNumber>.dkr.ecr.<region>.amazonaws.com/ibt-student', 'ecr:<region>:<credentialsId>
                    docker.withRegistry('https://630437092685.dkr.ecr.us-east-2.amazonaws.com/ibt-student', 'ecr:us-east-2:ibt-ecr') {
                    // build image
                    def myImage = docker.build("630437092685.dkr.ecr.us-east-2.amazonaws.com/ibt-student:latest")
                    // push image
                    myImage.push()
                    }
                }
            }
        }
        stage('Trivy Scan') {
            steps {
                sh 'trivy image 630437092685.dkr.ecr.us-east-2.amazonaws.com/ibt-student:latest'
            }
        }
    }
}
