pipeline {
    agent any
    environment {
     BRANCH_NAME = "${GIT_BRANCH.split("/")[1]}"
  }
    stages {
        stage('Verify branch') {
            steps {
                echo BRANCH_NAME
            }
        }
    }
}
            