pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: ' https://github.com/charliejhorn/8.2CDevSecOps.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Run Tests') {
            steps {
                sh 'npm test || true' // Allows pipeline to continue despite test failures

                // emailext body: 'Test Message',
                //     subject: 'Test Subject',
                //     to: 'chazzahorn@gmail.com'
            }
            post {
                always {
                    emailext(
                        subject: "${currentBuild.currentResult}: ${env.JOB_NAME} #${env.BUILD_NUMBER} — ${env.STAGE_NAME}",
                        body: """Stage: ${env.STAGE_NAME}
                            Build: ${env.JOB_NAME} #${env.BUILD_NUMBER}
                            Status: ${currentBuild.currentResult}
                            URL: ${env.BUILD_URL}""",
                        // body: """Stage: ${env.STAGE_NAME}
                        //     Build: ${env.JOB_NAME} #${env.BUILD_NUMBER}
                        //     Status: ${BUILD_STATUS}
                        //     URL: ${env.BUILD_URL}

                        //     Changes since last success (if any):
                        //     ${CHANGES_SINCE_LAST_SUCCESS}

                        //     Test summary (if using JUnit etc.):
                        //     ${TEST_COUNTS, format="Counts: total=${0}, passed=${1}, failed=${2}, skipped=${3}"}

                        //     Recent console output:
                        //     ${BUILD_LOG, maxLines=300}""",
                        to: 'chazzahorn@gmail.com',
                        attachLog: true,
                        compressLog: true
                    )
                }
            }
        }
        stage('Generate Coverage Report') {
            steps {
                // Ensure coverage report exists
                sh 'npm run coverage || true'
            }
        }
        stage('NPM Audit (Security Scan)') {
            steps {
                sh 'npm audit || true' // This will show known CVEs in the output
            }
            post {
                always {
                    emailext(
                        subject: "${currentBuild.currentResult}: ${env.JOB_NAME} #${env.BUILD_NUMBER} — ${env.STAGE_NAME}",
                        body: """Stage: ${env.STAGE_NAME}
                            Build: ${env.JOB_NAME} #${env.BUILD_NUMBER}
                            Status: ${currentBuild.currentResult}
                            URL: ${env.BUILD_URL}""",
                        // body: """Stage: ${env.STAGE_NAME}
                        //     Build: ${env.JOB_NAME} #${env.BUILD_NUMBER}
                        //     Status: ${BUILD_STATUS}
                        //     URL: ${env.BUILD_URL}

                        //     Recent console output:
                        //     ${BUILD_LOG, maxLines=200}""",
                        to: 'chazzahorn@gmail.com',
                        attachLog: true,
                        compressLog: true
                    )
                }
            }
        }
    }
}