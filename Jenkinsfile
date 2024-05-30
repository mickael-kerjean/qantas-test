pipeline {
    agent any
    stages {
        stage("Setup") {
            steps {
                git(
                    url: "git@github.com:mickael-kerjean/qantas-test.git",
                    credentialsId: "github-com-filestash-enterprise",
                    branch: "master"
                )
            }
        }
        stage("Build") {
            parallel {
                stage("qbr-login") {
                    when { 
                        changeset "qbr/qbr-login/**/*"
                    }
                    steps {
                        build job: "qantas-common", parameters: [string(name: "master", value: env.BRANCH_NAME)]
                        build job: "qantas-qbr-login", parameters: [string(name: "master", value: env.BRANCH_NAME)]
                    }
                }
                stage("qff-login") {
                    when { 
                        changeset "qff/qff-login/**/*"
                    }
                    steps {
                        build job: "qantas-common", parameters: [string(name: "master", value: env.BRANCH_NAME)]
                        build job: "qantas-qff-login", parameters: [string(name: "master", value: env.BRANCH_NAME)]
                    }
                }
            }
        }
    }
}
