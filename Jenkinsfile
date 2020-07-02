/* import shared library */
@Library('jenkins-shared-library')_

pipeline {
    agent none
    stages {
        stage('test golang') {
            agent { docker { image 'golang:1.13' } }
            environment {
                XDG_CACHE_HOME='/tmp/.cache'
                GOOS='linux'
                GOARCH='amd64'
        }        
            steps {
                script { checkgolang }
            }
        }
        stage('Check docker-compose syntax') {
            agent { docker { image 'docker/compose' } }
            steps {
                sh 'docker-compose -f \${WORKSPACE}/docker-compose.yml config'
            }
        }
        /*stage('Check Dockerfile syntax') {
            agent { docker { image 'hadolint/hadolint' } }
            steps {
                sh 'hadolint \${WORKSPACE}/Dockerfile'
            }
        }*/
    }
    post {
    always {
       script {
         /* Use slackNotifier.groovy from shared library and provide current build result as parameter */
         clean
         slackNotifier currentBuild.result
     }
    }
    }
    }
