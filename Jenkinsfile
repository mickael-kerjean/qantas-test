pipeline {
    agent any
    stages {
        stage("Setup") {
            steps {
                scm checkout
            }
        }
        stage("Build") {
            parallel {
                stage("qbr-login") {
                    when { changeset "qbr/qbr-login/**/*" }
                    steps { load "qbr/qbr-login/Jenkins/build.groovy" }
                }
                stage("qff-login") {
                    when { changeset "qff/qff-login/**/*" }
                    steps { load "qff/qff-login/Jenkins/build.groovy" }
                }
            }
        }
        state("Release") {
            steps {
                unstash "build"
                sh "ls -lah public"
                script {
                    readProperties(file: "public/.env").each {key, value -> env[key] = value }
                }
                sh "echo \"PATH: $APP\""
            }
        }
    }
}
