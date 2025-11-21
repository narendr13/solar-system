pipeline{
    agent any
    tools {
        nodejs 'nodejs-25-1-0'
    }
    environment {
        MONGO_URI=mongodb://98.93.176.139:27017
        MONGO_USER='myuser'
        MONGO_PSWD='mypassword'
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