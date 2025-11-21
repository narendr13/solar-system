pipeline{
    agent any
    tools {
        nodejs 'nodejs-25-1-0'
    }
    environment {
        MONGO_URI = "mongodb://172.31.76.116:27017/solar-system"
        MONGO_USER = "myuser"
        MONGO_PSWD = "mypassword"
    }

    stages {
        stage ('install dependencies'){
            steps {
                sh 'npm install'
                sh "echo MONGO_URI=$MONGO_URI"
            }
        }

        stage ('running unit tests'){
            steps{
                sh 'echo "Mocha exit code: $?"'
                sh 'npm test || true'
            }
        }
    }
}