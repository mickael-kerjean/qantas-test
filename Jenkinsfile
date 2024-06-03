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
                unstash "build"
                sh "ls -lah public"
                sh ". ./public/.env echo \"PATH: ${env.APP}\""
            }
        }
    }
}
