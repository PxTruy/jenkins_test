#!/usr/bin/env groovy
// See: https://github.com/jenkinsci/pipeline-examples/blob/master/docs/BEST_PRACTICES.md
// See: https://jenkins.io/doc/book/pipeline/
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
