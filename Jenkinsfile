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
            }
        }
        stage('dependencies scanning'){
            parallel{
                stage('dependency scanning npm audit'){
                    steps {
                        sh 'npm audit --audit-level=critical'
                    }
                }
                stage('dependency scanning using OWASP'){
                    steps {
                        dependencyCheck additionalArguments: '--scan . --format ALL --out ./ --prettyPrint --disableYarnAudit', odcInstallation: 'dependency-check'
                        dependencyCheckPublisher failedTotalCritical: 1, pattern: 'dependency-check-report.xml', skipNoReportFiles: true, stopBuild: true
                        publishHTML([allowMissing: true, alwaysLinkToLastBuild: true, icon: '', keepAll: true, reportDir: './', reportFiles: 'dependency-check-report.html', reportName: 'HTML Report', reportTitles: '', useWrapperFileDirectly: true])

                    }
                }
            }
        }

        stage ('running unit tests'){
            steps{
                sh 'echo "Mocha exit code: $?"'
                sh 'npm test || true'
            }
        }
        stage('code coverage'){
            steps{
                sh 'npm run coverage'
            }
        }
    }
}