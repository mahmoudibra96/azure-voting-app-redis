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
        stage('Start test app') {
            steps {
            pwsh(script: """
                //docker-compose up -d
               ./scripts/test_container.ps1
            """)
         }
         post {
            success {
               echo "App started successfully :)"
            }
            failure {
               echo "App failed to start :("
            }
         }
      }
      stage('Run Tests') {
         steps {
            pwsh(script: """
               pytest ./tests/test_sample.py
            """)
         }
      }
      stage('Stop test app') {
         steps {
            pwsh(script: """
               docker-compose down
            """)
         }
      }

    }
}
     