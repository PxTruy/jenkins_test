#!/usr/bin/env groovy
// See: https://github.com/jenkinsci/pipeline-examples/blob/master/docs/BEST_PRACTICES.md
// See: https://jenkins.io/doc/book/pipeline/

def getPRChangelog() {
    return sh(
            script: "git --no-pager diff origin/${params.target} --name-only",
            returnStdout: true
    ).split('\n')
}

pipeline {
    agent any
    options {
        buildDiscarder(logRotator(numToKeepStr: '100'))
        skipDefaultCheckout()
        timestamps()
    }
    stages {
        stage("Cleanup") {
            steps {
                deleteDir()
            }
        }
        stage("Checkout") {
            steps {
                // See: https://jenkins.io/doc/pipeline/steps/workflow-scm-step/
                script {
                    def scm_vars = checkout scm
                    println("" + scm_vars + "My scm vars are:")
                    println(currentBuild.changeSets)
                    def files = currentBuild.changeSets.stream().map { it.items }.toList()
                    println(files)
//                    sh "git diff --name-only ${scm_vars.GIT_PREVIOUS_COMMIT} ${scm_vars.GIT_COMMIT}"

                }

            }
        }
    }
    post {
        always {
            deleteDir()
        }
    }
}
