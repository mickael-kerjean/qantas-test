pipeline {
    agent any
    stages {
        stage("Build") {
            parallel {
                stage("qbr-login") {
                    when { changeset "qbr/qbr-login/**/*" }
                    steps { dir("qbr/qbr-login") { load "Jenkins/build.groovy" } }
                }
                stage("qff-login") {
                    when { changeset "qff/qff-login/**/*" }
                    steps { dir("qff/qff-login") { load "Jenkins/build.groovy" } }
                }
            }
        }
        stage("Release") {
            steps {
                // unstash "build"
                sh "mkdir public || true && echo \"APP=qbr/qbr-login\" > public/.env"
                script {
                    sh '''
                        set -a
                        source ./public/.env
                        set +a
                        env
                    '''
                }
                sh "echo \"PATH: ${env.APP}\""
            }
        }
    }
}
