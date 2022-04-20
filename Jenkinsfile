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
        stage('Dokcer Build') {
            steps {
                sh(script: "docker images -a")
                sh(script: """
                    cd azure-vote/
                    docker images -a
                    docker build -t jenkins-pipeline .
                    cd ..
                """)
            }
        }
    }
}
     