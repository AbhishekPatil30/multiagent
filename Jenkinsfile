pipeline {
    agent none
    environment {
        // Define environment variables
        STRING_VAR = 'Abhishek'
        CHOICE_VAR = 'Option1'
        BOOLEAN_VAR = true
    }
    triggers {
        // Periodic trigger and poll SCM
        pollSCM('H/15 * * * *')
        cron('H 2 * * *')
    }
    stages {
        stage('Stage 1') {
            agent { label 'agent1' }
            steps {
                echo "Running Stage 1"
                script {
                    def buildNumber = env.BUILD_NUMBER
                    echo "Build Number: ${buildNumber}"
                }
            }
        }
        stage('Stage 2') {
            agent { label 'agent1' }
            steps {
                echo "Running Stage 2"
                script {
                    def jobName = env.JOB_NAME
                    echo "Job Name: ${jobName}"
                    echo "Stage 2 completed successfully"
                }
            }
        }
        stage('Stage 3') {
            when {
                allOf {
                    branch 'main'
                    expression { return env.BOOLEAN_VAR.toBoolean() }
                }
            }
            agent { label 'agent2' }
            steps {
                echo "Running Stage 3"
                script {
                    def jenkinsUrl = env.JENKINS_URL ?: 'URL not defined'
                    echo "Jenkins URL: ${jenkinsUrl}"
                    echo "Stage 3 completed successfully"
                }
            }
        }
        stage('Stage 4') {
            agent { label 'agent2' }
            when {
                expression { return currentBuild.previousBuild?.result == 'SUCCESS' }
            }
            steps {
                echo "Running Stage 4"
                script {
                    def buildUrl = env.BUILD_URL ?: 'URL not defined'
                    echo "Build URL: ${buildUrl}"
                    echo "Stage 4 completed successfully"
                }
            }
        }
        stage('Use Credentials') {
            agent { label 'agent1' }
            steps {
                script {
                    // Use secret text securely without printing it
                    withCredentials([string(credentialsId: 'cred1', variable: 'SECRET_KEY')]) {
                        // Use SECRET_KEY in a secure context without echoing it
                        // For example, pass it to another process or service
                        // echo "Secret Key is used securely"
                    }

                    // Use username and password securely without printing them
                    withCredentials([usernamePassword(credentialsId: 'MY_CREDENTIALS', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                        // Use USERNAME and PASSWORD in a secure context without echoing them
                        // For example, authenticate to a service or pass them to another process
                        // echo "Username is used securely"
                    }
                }
            }
        }
    }
    post {
        always {
            emailext(
                to: 'linuxchutuku@gmail.com',
                subject: "Build ${env.BUILD_NUMBER} - ${currentBuild.currentResult}",
                body: """\
Build Details:

Build Number: ${env.BUILD_NUMBER}
Status: ${currentBuild.currentResult}
Check the Jenkins build console output for more details.
""",
                mimeType: 'text/plain',
                replyTo: 'linuxchutuku@gmail.com'
            )
        }
    }
}
