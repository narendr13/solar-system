pipeline{
    agent any
    tools {
        nodejs 'nodejs-25-1-0'
    }

    stages {
        stage ('install dependencies'){
            steps {
                sh 'npm install'
            }
        }

        stage ('running unit tests'){
            steps{
                sh 'npm test'
            }
        }
    }
}