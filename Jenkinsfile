#! /usr/bin/env groovy

node {

  def root = tool name: 'Go1.8', type: 'go'
  ws("${JENKINS_HOME}/jobs/${JOB_NAME}/builds/${BUILD_ID}/src/github.com/grugrut/golang-ci-jenkins-pipeline") {
      withEnv(["GOROOT=${root}", "GOPATH=${JENKINS_HOME}/jobs/${JOB_NAME}/builds/${BUILD_ID}/", "PATH+GO=${root}/bin"]) {
        env.PATH = "${GOPATH}/bin:$PATH"

        stage('Version Control and Branching Strategy') {
          deleteDir()
          checkout scm
          env.GIT_MSG = sh(
            script: "git log --pretty=%s -1",
            returnStdout: true
          ).trim()
          echo env.GIT_MSG
        }
        stage('commitArgs Parsing') {
          sh "go version"
          sh "go get ./..."
          sh "go install"
          sh "commitArgs ${env.GIT_MSG}"
        }
      }
