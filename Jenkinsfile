pipeline  {
  agent any
    
  stages    {
    stage('Test') {
                    steps {
                      echo "Build Number: $BUILD_NUMBER | Build ID: $BUILD_ID | BUILD_TAG: $BUILD_TAG"
                      echo "GIT_COMMIT: $GIT_COMMIT | JOB_NAME: $JOB_NAME"
                      sh 'echo "Test Success from Feature"'
                          }
                  }
   stage('Build') {
                     steps {
                        sh 'docker build -t todo-app-py:v.$BUILD_ID .'
                        echo "This is Build Based on Docker Image version v.$BUILD_ID"
                        echo "Build Success"
                           }
                  }

    stage('Login Dockerhub') {
                    steps {
                      withCredentials([string(credentialsId: 'kingstorm_dh', variable: 'DOCKER_TOKEN')]) {
                        sh "docker login -u himanshukingstorm -p $DOCKER_TOKEN"}                    
                        echo "Logged In Successfully into Dockerhub"
                          }
                             }
    
    stage('Push into Dockerhub') {
                    steps {
                      sh "docker tag todo-app-py:v.$BUILD_ID himanshukingstorm/todo-app-py:v.$BUILD_ID"
                      sh "docker push himanshukingstorm/todo-app-py:v.$BUILD_ID"

                        echo "This Push is Based on Docker Image as Version :v.$BUILD_ID"
                        echo "Pushed with Success into Dockerhub"
                          }
                                 }
    
    stage ('Update Manifest'){      
           steps {
//              sh "echo $BUILD_ID"
//              sh "sed -i 's/version/${BUILD_ID}/g' todo_app_deployment.yml"
             
             echo "YAML File Updated with current Build"
                 }
                             }
           
    stage('Deploy') {
                    steps {
                         withCredentials([file(credentialsId: 'pp', variable: 'my_var')]) {
                          script{
                            sh "envsubst < todo_app_deployment.yml | kubectl apply -f -"
//                             sh "kubectl --kubeconfig=$my_var apply -f todo_app_deployment.yml"
                            sh 'echo "Deployment is Successful"'
                                }
                                                                                          }
                          }  
                    }    
            }
       }
