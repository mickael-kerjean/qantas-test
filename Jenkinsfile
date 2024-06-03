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
                 env.TEST = sh(
                     script:
                     '''
                     echo "TEST"
                     ''',
                     returnStdout: true
                 )
                 sh """
                     echo ${env.BUILD_ID}
                     echo ${env.TEST}
                """
                // sh ". ./qbr/qbr-login/public/.env && echo ${env.APP}"
            }
        }
    }
}
