pipeline {
   agent any

   stages {
      stage('Verify Branch') {
         steps {
            git url: 'https://github.com/mahmoudibra96/azure-voting-app-redis.git' , branch: 'master' , credentialsId: 'githubtoken'
         }
      }
      stage('Docker Build') {
         steps {
            sh(script: 'docker images -a')
            sh(script: """
               cd azure-vote/
               docker images -a
               docker build -t jenkins-pipeline .
               docker images -a
               cd ..
            """)
         }
      }
      stage('Start test app') {
         steps {
            sh(script: """
               docker-compose up -d
               
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

      stage('Stop test app') {
         steps {
            sh(script: """
               docker-compose down
            """)
         }
      }
      stage('Pushing container to docker hub')  {
          steps {
              echo "Workspace is $WORKSPACE"
              dir("$WORKSPACE/azure-vote") {
              script {
                  docker.withRegistry('https://index.docker.io/v1/', 'dockerhub') {
                      def image = docker.build('mahmoudibrahem125/jencourse:latest')
                      image.push()
                  }
              }
              }
          }
          
      }


     stage('Trying palallel') {
        parallel {         
            stage('Run Trivy') {
         steps {
             sleep(time: 30 , unit: 'SECONDS')
          //  sh(script: """
           //      trivy mahmoudibrahem125/jencourse
            // """)
         }
      }
            stage('Echo any thing') {
         steps {
            echo "Hello"
         }
      }
        }
     }


           stage('Deploy to local kubernetes') {
        steps {
           script{
              kubernetesDeploy(configs: "azure-vote-all-in-one-redis.yaml" , kubeconfigId: "87147743-7e89-4213-9b8c-dede4ebd6c75")
           }
         }
      }

   }
}
