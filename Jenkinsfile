pipeline {
    agent {
        docker {
            label 'docker'
            image 'gradle:4.10.3-jdk8-alpine'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'gradle --no-daemon clean build'
            }
        }
        stage('Deploy') {
            environment {
                GITHUB = credentials('github_fsjovatsen')
            }
            when {
                tag pattern: "v\\d+\\.\\d+\\.\\d+(-\\w+-\\d+)?", comparator: "REGEXP"
            }
            steps {
                script {
                    VERSION = TAG_NAME[1..-1]
                }
                sh "echo Version is ${VERSION}"
                sh "gradle --no-daemon -Pversion=${VERSION} -Pgpr.user=${GITHUB_USR} -Pgpr.key=${GITHUB_PSW} publish"
            }
        }
    }
}
