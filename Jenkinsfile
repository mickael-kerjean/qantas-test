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
            script {
                env.APP = sh(script: 'echo "qbr/qbr-login"', returnStdout: true).trim()
            }
            steps {
                sh """
                    echo "APP: ${env.APP}"
                """
                // unstash "build"
                sh """
                #!/bin/bash
                export APP=qbr/qbr-login"
                echo ${env.APP}"
                """
            }
        }
    }
}
