pipeline{
    agent any
    tools {
        nodejs 'NodeJS-25-1-0'
    }

    stages {
        stage ('install dependencies'){
            steps {
                sh 'npm install'
            }
        }
    }
}