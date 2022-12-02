#!/usr/bin/env groovy
// See: https://github.com/jenkinsci/pipeline-examples/blob/master/docs/BEST_PRACTICES.md
// See: https://jenkins.io/doc/book/pipeline/
pipeline {
    agent {
        label 'docker'
    }
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
                    printMap(scm_vars, "My scm vars are:")
                    printMap(env.getEnvironment(), "My environment is:")
                    env.GIT_COMMIT = scm_vars.GIT_COMMIT
                    env.GIT_BRANCH = scm_vars.GIT_BRANCH
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
