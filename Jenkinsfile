pipeline{
    agent any
    tools {
        nodejs 'nodejs-25-1-0'
    }
    environment {
        MONGO_URI = "mongodb://172.31.76.116:27017/solar-system"
        MONGO_USER = credentials('DB_USERNAME')
        MONGO_PSWD = credentials('DB_PSWD')
    }

    options {
        disableConcurrentBuilds()
        retry(conditions: [agent()], count: 3)
        disableRestartFromStage()
        buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '1', numToKeepStr: '5')
    }
    parameters {
        string defaultValue: 'king ', description: 'for testing ', name: 'test'
        choice(name: 'ENV', choices: ['dev', 'staging', 'prod'], description: 'Choose environment')
    }
    stages {
        stage ('params display'){
            steps {
                echo "the parameter is ${params.ENV}"
            }
        }
        stage ('install dependencies'){
            steps {
                sh 'npm install'
            }
        }
        stage('dependencies scanning'){
            parallel{
                stage('dependency scanning npm audit'){
                    steps {
                        sh 'npm audit || true'
                    }
                }
                stage('dependency scanning using OWASP'){
                    steps {
                        dependencyCheck additionalArguments: '--scan . --format ALL --out ./ --prettyPrint --disableYarnAudit', odcInstallation: 'dependency-check'
                        dependencyCheckPublisher failedTotalCritical: 50, pattern: 'dependency-check-report.xml', skipNoReportFiles: true, stopBuild: true

                        publishHTML([allowMissing: true, alwaysLinkToLastBuild: true, icon: '', keepAll: true, reportDir: './', reportFiles: 'dependency-check-report.html', reportName: 'HTML Report', reportTitles: '', useWrapperFileDirectly: true])

                        junit allowEmptyResults: true, keepProperties: true, keepTestNames: true, skipMarkingBuildUnstable: true, testResults: 'dependency-check-junit.xml'
                    }
                }
            }
        }

        stage ('running unit tests'){
            steps{
                sh 'echo "Mocha exit code: $?"'
                sh 'npm test || true'
                junit allowEmptyResults: true, keepProperties: true, keepTestNames: true, skipMarkingBuildUnstable: true, skipOldReports: true, testResults: 'test-results.xml'
            }
        }
        stage('code coverage'){
            steps{
                catchError(buildResult: 'SUCCESS', message: 'oops! it\'s been failed', stageResult: 'UNSTABLE') {
                    sh 'npm run coverage'
                }
                publishHTML([allowMissing: true, alwaysLinkToLastBuild: false, icon: '', keepAll: false, reportDir: 'coverage/lcov-report', reportFiles: 'index.html', reportName: 'Coverage HTML Report', reportTitles: '', useWrapperFileDirectly: true])
            }
        }
        stage('build docker image'){
            steps{
                sh 'docker -v'
            }
        }
    }
}