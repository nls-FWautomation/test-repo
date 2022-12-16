
pipeline {
  agent any
  stages {
    stage('version') {
      steps {
        sh 'python3 --version'
      }
    }
    stage('checkout') {
      steps {
        sh 'git checkout main'
        sh('git pull')
        sh('pwd')
        sh('git status')
        sh('git branch')
      }
    }
    stage('newBranch') {
      steps {
        sh 'git checkout -b build_automation_${BUILD_NUMBER}'
        sh('pwd')
        sh('git branch')
        sh('git status')
      }
    }
    stage('buildFile') {
      steps {
        sh 'touch ${BUILD_NUMBER}.txt'
        sh 'python3 /Users/fw_build_server/hello.py'
        sh('git status')
        sh('git branch')
      }
    }
    stage('commitChanges') {
      steps {
        sh('pwd')
        sh('git branch')
        sh('git status')
        sh('git add --all')
        sh('git status')
        sh 'git commit -am "Test Commit-${BUILD_NUMBER}"' 
        sh('git status')
        sh('git branch')
      }
    }
    stage('deploy') {
      steps {
        // credentialsId here is the credentials you have set up in Jenkins for pushing
        // to that repository using username and password.
        withCredentials([usernamePassword(credentialsId: 'GitHub-PAT', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
          sh('git status')
          sh('git branch')
          sh('pwd')
          sh("git tag -a ${BUILD_TAG} -m 'Jenkins build ${BUILD_NUMBER}'")
          sh('git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/nls-FWautomation/test-repo.git')
        }

      }
    }
  }
  post {
    always {
      echo 'post:  where to delete dir'
      //deleteDir()
      cleanWs()
    }
    // success {
    //   mail to:"dcraft@nautilus.com", subject:"SUCCESS: ${currentBuild.fullDisplayName}", body: "Yay, we passed."
    // }
    // failure {
    //   mail to:"dcraft@nautilus.com", subject:"FAILURE: ${currentBuild.fullDisplayName}", body: "Boo, we failed."
    // }
  }
}