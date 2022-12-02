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

                    for (int i = 0; i < currentBuild.changeLogSets.size(); i++) {
                        def entries = currentBuild.changeLogSets[i].items
                        for (int j = 0; j < entries.length; j++) {
                            def entry = entries[j]
                            echo "${entry.commitId} by ${entry.author} on ${new Date(entry.timestamp)}: ${entry.msg}"
                            def files = new ArrayList(entry.affectedFiles)
                            for (int k = 0; k < files.size(); k++) {
                                def file = files[k]
                                echo " ${file.editType.name} ${file.path}"
                            }
                        }
                    }
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
