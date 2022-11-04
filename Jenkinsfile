
pipeline {
  agent any
  stages {
    stage('version') {
      steps {
        sh 'python3 --version'
      }
    }
    stage('hello') {
      steps {
        sh 'git checkout main'
        sh('git pull')
        sh 'python3 /Users/fw_build_server/hello.py'
        sh('git status')
        sh('git add .')
        sh('git status')
        sh 'git commit -m "Test Commit-${BUILD_NUMBER}"' 
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
          sh('git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/nls-FWautomation/test-repo.git')
        }

      }
    }
  }
  post {
    always {
      echo 'post:  where to delete dir'
      //deleteDir()
    }
    // success {
    //   mail to:"dcraft@nautilus.com", subject:"SUCCESS: ${currentBuild.fullDisplayName}", body: "Yay, we passed."
    // }
    // failure {
    //   mail to:"dcraft@nautilus.com", subject:"FAILURE: ${currentBuild.fullDisplayName}", body: "Boo, we failed."
    // }
  }
}