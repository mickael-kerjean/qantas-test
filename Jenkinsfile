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
                script {
                    env.APP = sh(script: 'cat "qbr/qbr-login/public/.app"', returnStdout: true).trim()
                }
                sh """
                    echo "APP: ${env.APP}"
                """
            }
        }
    }
}
